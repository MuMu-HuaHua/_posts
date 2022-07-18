title: ETL
date: 2022-07-09 18:30:09
category:
- [Code,BD]
tags:
- Linux
- ETL
- BD
  
toc: true
---
ETL(Extract、Transform、Load)，大数据场景下，数据量越大，计算逻辑愈发复杂，数据清洗需放在运算能力更强的分布式计算引擎中完成，ETL也就变成了ELT（Extract-Load-Transform）
<!--more-->
### 主流ETL工具
- Sqoop（Apache）
- DataX(阿里)
- Kettle（纯开源）
- Fivetran(云数仓)
### 加载策略
- 全量
- 增量
- 流式（Kafka）
使用kafka，消费mysql binlog日志到目标库，源表和目标库是1：1的镜像。
无论是全量还是增量的方式，都会浪费多余的存储或通过计算去重，得到最新的全量数据。为解决这一问题，可以使用kafka的数据同步方案，源表变化一条，目标表消费一条，目标表数据始终是一份最新全量数据，且为实时同步的。


Fivetran的服务系统基于云构建，用户侧分析也是云上的数仓。 Hadoop、AWS Athena走数据湖线路，可以快速完成初期系统的搭建，但可能因为缺乏数据schema规划、缺少计算下推的辅助，牺牲了一定的分析效率。 以AWS Redshift、Hive为代表的数仓，提供高效率的压缩存储以及存储、计算的一体化，提升了分析效率，但系统搭建依赖前期表和schema设计，以及在将来schema变化时伴随着维护成本。 Fivetran选择适配多数仓系统，由用户根据业务场景自主选择用什么做分析。使用SQL（被广泛支持的数仓语言）统一用户的Transform、Analytics使用体验


### sqoop



#### install


