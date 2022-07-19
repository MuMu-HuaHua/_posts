---
title: 命令行（三）
categories:  [Code,shell]
tags:
  - Linux
  - DataScience
toc: true
abbrlink: 6885c91c
date: 2022-07-14 23:20:09
---

## 管理数据工作流
使用命令行进行数据处理是具有探索性质的，还是需要做一个记录，以便后续梳理。使用Drake进行管理。
<!--more-->
### 安装
``` sh
sudo apt-get install openjdk-6-jdk
sudo apt-get install leiningen
# cd /Data/commandline
git clone https://github.com/Factual/drake.git
cd drake
# export HTTP_CLIENT="wget --no-check-certificate -O"
lein uberjar
mv drake.jar ~/.bin/
cd ~/.bin/
java -jar drake.jar
# 安装Drip，这是JVM的启动器，比以java命令启动得更快
# Drip能够加速Java是因为它在JVM运行一次之后保留了JVM实例
# 所以只有第二次之后的运行才会加速
# 创建启动脚本
cd ~/.bin
cat << 'EOF' > drake
#!/bin/bash
drip -cp $(dirname $0)/drake.jar drake.core "$@"
EOF
chmod +x drake

# 验证
drake --version

"i" in them
out.csv <- in.csv
  grep i $INPUT > $OUTPUT

Artem,Boytsov,artem
Aaron,Crow,aaron
Alvin,Chyan,alvin
Maverick,Lou,maverick
Vinnie,Pepi,vinnie
Will,Lao,will
```


安装配置尚未解决，Skip


## 数据探索

### 检查数据
``` sh
head file.csv | csvlook
# cat 会一次性全部输出数据
# less的优点在于，它并不会把整个文件都读进内存
# 处于less状态中，可以通过按下空格键向下滚动一整屏
# 横向滚动则可以通过按下向左和向右键实现
# 按g和G可以分别跳到文件的开头和末尾。
# 按q可以退出less
# 也可以将csvlook加入管道
less -s file.csv
< file.csv csvlook | less -S

```
### 统计描述 & 模型

### 可视化



## 并行管道（GNU Parallel）
常见的遍历的数据类型：数字、文本行和文件

### 遍历
数字遍历
bc——基本上是一个在命令行中将公式输入到管道的计算器
``` sh
#  Bash有一个特性叫作括号扩展，它会将{0..100..2}变成一列由空格分隔的数字
# 在执行echo之前，Shell会把$i转换成它的值
# 注意在do和done之间可以有多个命令
for i in {0..100..2} 
do
echo "$i^2" | bc 
done | tail 
```
行遍历

```sh
# 重定向也可以放在while之前
while read line ①
do
echo "Sending invitation to ${line}."
done
```

文件遍历
``` sh
for filename in *.csv
do
echo "Processing ${filename}."
done

# 使用find和parallel
# -print0选项允许处理find输出的程序正确解释包含有换行符或其他类型空白字符的文件名
#如果完全确定这些文件名并不包含特殊字符，如空格和回车，那么可以省略-print0和-0选项
find data -name '*.csv' -print0 | parallel -0 echo "Processing {}"
```
如果待处理的列表过于复杂，也可以把结果保存到一个临时文件中，然后使用这个方法对文件中的行进行遍历

### 并行处理

``` sh
# $RANDOM是一个内置的Bash函数，它可以返回一个0到32767之间的伪随机数

#  parallel --version | head -n 1

```
###  分布式

## 数据建模
常见的四类建模算法：降维、聚类、回归、分类。前两者为无监督、后两者为有监督

### 用Tapkee降维
降维的目的是把高维数据映射到低维空间，重点在于要在低维映射中让相似的数据保持相近.(主要使用PCA\t-SNE)。Tapkee是一个用于降维的C++模版库，包含的降维算法：局部线性嵌入、Isomap、多维标度、PCA、t-SNE
``` sh
apt-get install cmake
curl -sL https://github.com/lisitsyn/tapkee/archive/master.tar.gz > \
tapkee-master.tar.gz
tar -xzf tapkee-master.tar.gz
cd tapkee-master
mkdir build && cd build
cmake ..
make
```
 #### PCA


 #### t-SNE
t分布随机邻近嵌入