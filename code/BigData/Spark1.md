title: Spark
date: 2022-07-09 18:30:09
categories:
- [Code,BD]
tags:
- Linux
- Spark
- BD
  
toc: true
---

### 常用组件
Spark core、Spark SQL 、Spark Streaming 、Mlib

<!--more-->

- 一点概念
  Arg|Mean
  ---|--
  RDD|是分布式内存的一个抽象概念，提供了一种高度受限的共享内存模型。
  DAG|反映RDD之间的依赖关系。
  Executor|是运行在工作节点（Worker Node）上的一个进程，负责运行任务，并为应用程序存储数据。
  application|用户编写的Spark应用程序。
  task|运行在Executor上的工作单元。
  job|一个作业（Job）包含多个RDD及作用于相应RDD上的各种操作。
  stage|是作业的基本调度单位，一个作业会分为多组任务（Task），每组任务被称为“阶段”（Stage）或者也被称为“任务集”。

在 Spark 中，一个应用（Application）由一个任务控制节点（Driver）和若干个作业（Job）构成，一个作业由多个阶段（Stage）构成，一个阶段由多个任务（Task）组成。

当执行一个应用时，任务控制节点会向集群管理器（Cluster Manager）申请资源，启动 Executor，并向 Executor 发送应用程序代码和文件，然后在 Executor上执行任务，运行结束后执行结果会返回给任务控制节点，或者写到HDFS或者其他数据库中。



