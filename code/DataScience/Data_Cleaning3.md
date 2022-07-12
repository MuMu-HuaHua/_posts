title: 数据清洗(3)
date: 2022-07-11 22:30:09
categories:
- [Code,DataScience]
- [Statistic,Theory]

tags: 
- 数据清洗
- SQL
- Linux
- Python
---

### Wash

#### text
- 原始推文中可能含有high-ASCII或Unicode编码的撇号（有时候也称为智能引号）
- 这些数据在向低位字符集转换的时候，比如纯ASCII，特殊风格的撇号就会被简单地转换成?
<!--more-->
- 在MySQL中，转义字符两种：
    - 反斜线
    - 单引号本身
- 拆分 
#### date
- MySQL可以处理date、time或是datetime数据
- 流程
    - 修改原来的数据表并加入一个新的字段（存放新生成的日期时间信息）
    - 针对每一行数据执行更新语句（对字段进行格式化）



####  Project
``` python
# connect to mysql
pip install PyMySQL
import pymysql
 
# 打开数据库连接
db = pymysql.connect(host='localhost',
                    user='localuser',
                    password='local2022-',
                    database='sentiment140')
 
# 使用 cursor() 方法创建一个游标对象 cursor
cursor = db.cursor()
 
# 使用 execute()  方法执行 SQL 查询 
cursor.execute("SELECT VERSION()")
 
# 使用 fetchone() 方法获取单条数据.
data = cursor.fetchone()
 
print ("Database version : %s " % data)
 
# 关闭数据库连接
db.close()
```

``` SQL
USE sentiment140;

CREATE TABLE sentiment140 (
    ->  polarity enum('0','2','4') DEFAULT NULL,
    ->  id int(11) PRIMARY KEY,
    ->  date_of_tweet varchar(28) DEFAULT NULL,
    ->  query_phrase varchar(10) DEFAULT NULL,
    ->  user varchar(10) DEFAULT NULL,
    ->  tweet_text varchar(144) DEFAULT NULL
    -> )  ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

# LOAD DATA INFILE 'cleanedTestData.csv' 
LOAD DATA LOCAL INFILE 'cleanedTestData.csv' 
INTO TABLE sentiment140
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n';

# 拆分   
CREATE TABLE IF NOT EXISTS sentiment140_mentions (
    id int(11) NOT NULL AUTO_INCREMENT,
    tweet_id int(11) NOT NULL,
    mention varchar(144) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
CREATE TABLE IF NOT EXISTS sentiment140_hashtags (
    id int(11) NOT NULL AUTO_INCREMENT,
    tweet_id int(11) NOT NULL,
    hashtag varchar(144) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
CREATE TABLE IF NOT EXISTS sentiment140_urls (
    id int(11) NOT NULL AUTO_INCREMENT,
    tweet_id int(11) NOT NULL,
    url varchar(144) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```
##### 提取标签
- 标签以符号#开始
- 标签单词会紧紧跟随符号#
- 标签中可以包含下划线，但不可以包含空格或其他标点符号

``` SQL
# 在不区分大小写的情况下匹配Unicode字符
if (preg_match_all(
    "/(#\pL+)/iu",
    $row["tweet_text"],
    $hashtags
    ))

```

##### 提取URL
- http或https
- file://
- torrent
``` SQL
if (preg_match_all(
    "!https?://\S+!",
    $row["tweet_text"],
    $urls
))
urls = array();

pattern = '#\b(([\w-]+://?|www[.])[^\s()<>]+(?:\([\w\d]+\)|([^[:punct:]\s]|/)))#';
while($row = mysqli_fetch_array($select_result))
{
    echo "<br/>working on tweet id: " . $row["id"];
    if (preg_match_all(
        $pattern,
        $row["tweet_text"],
        $urls
    ))
    {
        foreach ($urls[0] as $name)
        {
            echo "<br/>----url: ".$name;
            $insert_query = "INSERT into sentiment140_urls (id,
            tweet_id, url)
                    VALUES (NULL," . $row["id"] . ",'$name')";
            echo "<br />$insert_query";
            $insert_result = mysqli_query($dbc, $insert_query);
            // 如果查询失败则终止处理
            if (!$insert_result)
                die ("INSERT failed! [$insert_query]" .
                mysqli_error());
        }
    }
}
```
##### 查询表


#### 记录文档
每条SQL语句。
- 每个Excel函数和文本编辑器程序，如果有必要可以加入演示截图。
- 每个脚本程序。
- 关于历史操作的笔记和注释。
> 还有一种比较好的思路就是在每个阶段都创建一个备份表

- md文件规范
    -  文件名和其所在的包名
    -  文件作者或是所有协作者的名字，组织和所在地
    -  文件发布日期
    -  文件版本号，及其早期版本的所在位置
    -  文件的用途
    -  原始数据来源，数据经过的处理
    -  文件格式与组织方法，如各个字段和它们对应的含义
    -  文件使用条款和许可协议

- 实体关系图（Entity-Relationship Diagram，ERD）