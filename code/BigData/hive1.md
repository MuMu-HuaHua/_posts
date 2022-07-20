---
title: Conception Of Hive
categories: [Code,BD]
tags:
  - Hive
  - BD
abbrlink: cba7b740
date: 2022-07-20 18:30:09
banner_img: https://cdn.jsdelivr.net/gh/MuMu-HuaHua/picgo/202207160917401.jpg
toc: true
---
<!--more-->
### 基本概念
Hive是基于hadoop的数仓工具，可以将——==数据化结构数据文件映射为==一张表，同时==提供类SQL查询功能==。本质上是将HQL转换为Mapreduce程序。

#### 架构
| 组件                      | 定义                                              |
| ------------------------- | ------------------------------------------------- |
| 用户接口(client)          | CLI(hive shell)、JDBC/ODBC(java访问hive)、 <br>WebUI(网页访问，主要通过浏览器)    |
| 元数据<br>(Metastore)     | 包括表名，表所属2的数据库、表的拥有者、列/分区字段、表的类型（是否为外部表）、表所在目录等<br>                                                           |
| Hadoop                    | 使用 HDFS 进行存储，使用 MapReduce 进行计算 |
| 驱动器(Driver)            | 解析器、编译器、优化器、执行器|
| 解析器<br>（SQL Parser）  | 将 SQL 字符串转换成抽象语法树 AST，这一步一般都用第三方工具库完成，比如 antlr；<br>对 AST 进行语法分析，比如表是否存在、字段是否存在、SQL 语义是否有误。 |
| 编译器（Physical Plan）   | 将 AST 编译生成逻辑执行计划|
| 优化器（Query Optimizer） | 对逻辑执行计划进行优化|
| 执行器（Execution）   | 把逻辑执行计划转换成可以运行的物理计划。对于 Hive 来说，就是 MR/Spark|

Hive通过为用户提供交互接口，接受用户的指令（SQL，用户在这一过程中创建映射关系），使用自己的Driver，结合元数据（MetaData），将指令翻译为MapReduce，提交到Hadoop执行，最后将返回的结果输出到用户交互接口。Hive构建在基于静态批处理的Hadoop之上，由于Hadoop通常都有较高的延迟并且在作业提交和调度的时候需要大量的开销。因此，Hive并不适合那些需要低延迟的应用，它最适合应用在基于大量不可变数据的批处理作业。Hive将元数据存储在关系型数据库（RDBMS）中，比如MySQL、Derby中。

Hive有三种模式连接到数据：
- 单用户模式
- 多用户模式
- 远程服务模式。（内嵌模式、本地模式、远程模式）。

#### Hive基本数据类型
| Hive数据类型 | Java数据类型       | 长度            | 例子         |
| ------------ | ------------------ | --------------- | ------------ |
| TINYINT      | byte               | 1byte有符号整数 | 20           |
| SMALLINT     | short              | 2byte有符号整数 | 20           |
| INT          | int                | 4byte有符号整数 | 20           |
| BIGINT       | long               | 8byte有符号整数 | 20           |
| BOOLEAN      | boolean            | 布尔类型        | TRUE,FALSE   |
| FLOAT        | float	单精度浮点数 | 3.14159         |
| DOUBLE       | double             | 双精度浮点数    | 3.14159      |
| STRING       | string             | 字符            | "hello hive" |
| TIMESTAMP    | 时间类型           |                 |              |
| BINARY       | 字节数组           |                 |

{% note style %}
...对于 Hive 的 String 类型相当于数据库的 varchar 类型，该类型是一个可变的字符串，不过并不能声明其中最多能存储多少个字符，理论上可以存储 2GB 的字符数
{% endnote %}

#### hive集合数据类型
ARRAY、MAP 和 STRUCT：
- ARRAY 和 MAP 与 Java 中的Array 和 Map 类似
- STRUCT 与 C 语言中的 Struct 类似，它封装了一个命名字段集合，复杂数据类型允许任意层次的嵌套

表字段释义：
- `row format delimited fields terminated by ',' `-- 列分隔符
- `collection items terminated by '_'` --MAP STRUCT 和 ARRAY 的>>分隔符(数据分割符号)
- `map keys terminated by ':'` -- MAP 中的 key 与 value 的分隔符
- `lines terminated by '\n';` -- 行分隔符
- `row format serde 'org.apache.hadoop.hive.serde2.JsonSerDe'`; -- 已经封装完成的序列化和反序列化数据格式;如JsonSerDe，可以按json格式进行切割并将对应key的value值匹配到对应的字段上。


``` raw
create table test(
name string,
friends array<string>,
children map<string, int>,
address struct<street:string, city:string>
)
row format delimited 
fields terminated by ','
collection items terminated by '_'
map keys terminated by ':'
lines terminated by '\n';

{
  "name": "songsong",
  "friends": ["bingbing" ,  "lili"] , //列表 Array, 
  "children": { //键值 Map,
  "xiao song": 18 ,
  "xiaoxiao song": 19
  }
  "address": { //结构 Struct,
  "street": "hui long guan" ,
  "city": "beijing" 
 } }
```
### 安装
前提：Hadoop与MySQL已经安装好
流程：
1. 下载解压
   下载apache-hive-2.3.8-bin.tar.gz和mysql-connector-java-8.0.25.jar
  ```  sh
  cd /data/hive1
  wget https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-2.3.8/apache-hive-2.3.8-bin.tar.gz
  wget https://dev.mysql.com/downloads/file/?id=504646
   ```
2. 配置环境变量
  ``` sh
  vim ~/.bashrc
  #hive config  
  export HIVE_HOME=/apps/hive  
  export PATH=$HIVE_HOME/bin:$PATH
  ```
3. 配置MySQL连接包
  解压jar包到hive的lib目录下
4. 配置HIve
  javax.jdo.option.ConnectionURL：数据库链接字符串。
  javax.jdo.option.ConnectionDriverName：连接数据库的驱动包。
  javax.jdo.option.ConnectionUserName：数据库用户名。
  javax.jdo.option.ConnectionPassword：连接数据库的密码。
  ``` sh
  cd /apps/hive/conf  
  touch hive-site.xml
  vim hive-site.xml
  ```
  配置xml文件
    ``` xml
    <property>
    #javax.jdo.option.ConnectionDriverName：连接数据库的驱动包。
      <name>javax.jdo.option.ConnectionDriverName</name>
      <value>com.mysql.jdbc.Driver</value>
    </property>
    <property>
    #javax.jdo.option.ConnectionUserName：数据库用户名。
      <name>javax.jdo.option.ConnectionUserName</name>         <value>root</value>
    </property>
    <property>
    #javax.jdo.option.ConnectionPassword：连接数据库的密码。
      <name>javax.jdo.option.ConnectionPassword</name>
      <value>12345</value>                                                                     
    </property>
      <property>
        <name>hive.server2.thrift.port</name>
        <value>10000</value>
      </property>
    <property>
      <name>hive.server2.thrift.bind.host</name>
      <value>127.0.0.1</value>
    </property>
  ```
  将hive-env.sh.template重命名为hive-env.sh
  `mv /apps/hive/conf/hive-env.sh.template  /apps/hive/conf/hive-env.sh `
  配置`hive-env.sh`，追加Hadoop的路径以及Hive配置文件的路径
  ``` sh
  HADOOP_HOME=/apps/hadoop 
  export HIVE_CONF_DIR=/apps/hive/conf
  ```
5. 测试安装
   - 启动mysql
   - 启动Hadoop   `/apps/hadoop/sbin/start-all.sh  `
   - 启动hive     `hive `

### 安装报错集合
1. `Exception in thread “main” java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;Ljava/lang/Object;)V`
  - 原因：hadoop和hive的lib下有一个guava开头的jar包不匹配。
  - 解决：查看所在路径下的guava开头的jar,选择版本高的去覆盖低版本
  - ```
    cd /apps/hadoop/share/hadoop/common/lib/
    cd /apps/hive/lib/
    ```
  











