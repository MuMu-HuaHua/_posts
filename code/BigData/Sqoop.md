title: Sqoop_Record
date: 2022-07-19 18:30:09
category:
- [Code,BD]
tags:
- Linux
- Sqoop
- BD
  
toc: true
---

sqoop 是在==Hadoop 和关系数据库服务器之间传送数据==的工具，使用时需要在节点上安装。核心功能就是数据的导入导出和迁入迁出。
<!--more-->
工作机制是将导入或导出命令翻译成 MapReduce 程序来实现，在翻译出的 MapReduce 中主要是对 InputFormat 和 OutputFormat 进行定制
- 导入数据
  - 从MySQL，Oracle 导入数据到 Hadoop 的 HDFS、HIVE、HBASE 等数据存储系统
- 导出数据
  -  Hadoop 的文件系统中导出数据到关系数据库 mysql 等 Sqoop 的本质还是一个命令行工具

sqoop本质就是迁移数据， 迁移的方式是把sqoop的迁移命令转换成MR程序
hive本质就是执行计算，依赖于HDFS存储数据，把SQL转换成MR程序
### install
``` sh
tar -zxvf sqoop-1.4.7.bin__hadoop-2.0.4-alpha.tar.gz -C /Data/
cd /Data
mv sqoop-1.4.7.bin__hadoop-2.0.4-alpha/ sqoop-1.4.7
cd /Data/sqoop-1.4.7
# ls -al
mv sqoop-env-template.sh  sqoop-env.sh

# edit config_files
vim sqoop-env.sh 

export HADOOP_COMMON_HOME=/home/hadoop/apps/hadoop-2.7.5

#Set path to where hadoop-*-core.jar is available
export HADOOP_MAPRED_HOME=/home/hadoop/apps/hadoop-2.7.5

#set the path to where bin/hbase is available
export HBASE_HOME=/home/hadoop/apps/hbase-1.2.6

#Set the path to where bin/hive is available
export HIVE_HOME=/home/hadoop/apps/apache-hive-2.3.3-bin

#Set the path for where zookeper config dir is
export ZOOCFGDIR=/home/hadoop/apps/zookeeper-3.4.10/conf
```














