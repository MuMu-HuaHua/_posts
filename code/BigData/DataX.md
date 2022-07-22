---
title: OPeration Of Hive
categories: [Code,BD]
tags:
  - Hive
  - BD
  - DataX
date: 2022-07-24 18:30:09
banner_img: https://cdn.jsdelivr.net/gh/MuMu-HuaHua/picgo/202207220000990.jpg
toc: true
---

### 架构
[DataX](https://github.com/alibaba/DataX) 是阿里巴巴开源的一个==异构数据源离线同步工具==，致力于实现包括关系型数据库(MySQL、Oracle等)、HDFS、Hive、ODPS、HBase、FTP等各种异构数据源之间稳定高效的数据同步功能。DataX将复杂的网状的同步链路变成了星型数据链路，==DataX作为中间传输载体负责连接各种数据源。__当需要接入一个新的数据源的时候，只需要将此数据源对接到DataX，便能跟已有的数据源做到无缝数据同步__。

#### 框架 
DataX使用离线数据同步框架，采用Framework + plugin架构构建。将数据源读取和写入抽象成为Reader/Writer插件，纳入到整个同步框架中。

![框架](https://cdn.jsdelivr.net/gh/MuMu-HuaHua/picgo/202207220010744.webp)
- Reader：数据采集模块，负责采集数据源的数据，将数据发送给Framework。
- Writer：数据写入模块，负责不断向Framework取数据，并将数据写入到目的端。
- Framework：用于连接reader和writer，作为两者的数据传输通道，并处理缓冲，流控，并发，数据转换等核心技术问题。

#### 运行流程
一个周期内的运行流程：
- Job：单个数据同步的作业，称为一个Job，一个Job启动一个进程。
- Task根据不同数据源的切分策略：一个Job会切分为多个Task，Task是Datax作业的最小单元，每个Task负责一部分数据的同步工作。
- TaskGroup：Schedule调度模块会对Task进行分组，每个Task组称为个TaskGroup。每个TaskGroup负责以一定的并发度运行其所分得的Task，单个TaskGroup的并发度为5。
- Reader→Channel-→Writer：每个Task启动后，都会固定启动Reader→Channel→Writer的线程来完成同步工作。
![流程](https://cdn.jsdelivr.net/gh/MuMu-HuaHua/picgo/202207220917890.webp)

#### 安装
前置：
- java
- python
- Apache-Maven(可选，使用maven进行源码打包，不过spark也会用到maven)
[DataX](https://datax-opensource.oss-cn-hangzhou.aliyuncs.com/datax.tar.gz)
``` sh
wget https://datax-opensource.oss-cn-hangzhou.aliyuncs.com/datax.tar.gz

tar -zxvf datax.tar.gz -C /opt/modules/
cd /Data/db-utils/datax/bin
# python datax.py ./stream2stream.json
python datax.py /Data/db-utils/datax/job/job.json
```


### 基本使用
用户需根据同步数据的数据源和目的地选择相应的Reader和Writer，并将Reader和Writer的信息配置在一个json文件中，然后执行如下命令提交数据同步任务,基本提交形式——`python bin/datax.py path/to/your/job.json`
#### 配置文件
``` sh
# 文件模板
python bin/datax.py -r mysqlreader -w hdfswriter
# json最外层是一个job，job包含setting和content
# setting用于对整个job进行配置
# content用户配置数据源和目的地。
# Datax对不同数据源的支持
# https://github.com/alibaba/DataX/blob/master/README.md

# 同步MySQL数据到HDFS案例


```








### 优化
#### 控速
DataX3.0提供了包括通道(并发)、记录流、字节流三种流控模式，可以控制作业速度，让作业在数据库可以承受的范围内达到最佳的同步速度。
关键优化参数：
| 参数         | 说明                |
| ------------ | -----------------  |
|job.setting.speed.channel | 并发数  |
|job.setting.speed.record  | 总record限速 |
|job.setting.speed.byte    | 总byte限速   |
|core.transport.channel.speed.record | 单个channel的record限速，默认值为10000（10000条/s） |
| core.transport.channel.speed.byte   | 单个channel的byte限速，默认值1024*1024（1M/s）      |

{% note info %}
1. 若配置了总record限速，则必须配置单个channel的record限速
2. 若配置了总byte限速，则必须配置单个channe的byte限速
3. 若配置了总record限速和总byte限速，channel并发数参数就会失效。因为配置了总record限速和总byte限速之后，实际channel并发数是通过计算得到的,计算公式为:min(总byte限速/单个channel的byte限速，总record限速/单个channel的record限速)
{% endnote %}


#### 内存调整
当提升DataX Job内Channel并发数时，内存的占用会显著增加，因为DataX作为数据交换通道，在内存中会缓存较多的数据。例如Channel中会有一个Buffer，作为临时的数据交换的缓冲区，而在部分Reader和Writer的中，也会存在一些Buffer，为了防止OOM等错误，需调大JVM的堆内存。==建议将内存设置为4G或者8G，这个也可以根据实际情况来调整。==
调整JVM xms xmx参数：
- 直接更改datax.py脚本；
- 在启动的时候，加上对应的参数`python datax/bin/datax.py --jvm="-Xms8G -Xmx8G" /path/to/your/job.json`








