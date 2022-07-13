title: 数据清洗(2)
date: 2022-07-9 22:30:09
categories:
- [Code,DataScience]
- [Statistic,Theory]

tags: 
- 数据清洗
- SQL
- Linux
- Python
---

## 传统工具
### EXCEL
#### 数据清洗
1. 文本分列
    - 数据-分列-分割符/指定列宽
    - 在分列前也可以考虑替换掉一些符号，方便通过分割符进行划分
    - trim()函数可以去除字符串前面的空格字符
1. 一对多转换为一对一（两列）
<!--more-->

### MYSQL
1. 导入数据
    - LOAD DATA IN FILE的命令把数据从分隔文件加载到数据库中
    - 支持自定义分隔符号
    ``` SQL
    load data local infile 'myFile.csv'
        into table freenode_topics
        fields terminated by ','
        (dateOfTopic, channel, numUsers, message);

    mysql -u -p -h
    use databasename;
    source inserts.sql
    ```

#### HTML

- 行分隔模型
- 树形结构

#### Python和正则表达式
- 从html页面确定需要的数据
- python


``` python
# python2
import re
import io
row = []
infile = io.open('DjangoLog.html', 'r', encoding='utf8')
outfile = io.open('csvfile.csv', 'a+', encoding='utf8')
for line in infile:
    pattern = re.compile(ur'<li class=\"le\" rel=\"(.+?)\"><a href=\"#(.+?)\" name=\"(.+?)<\/span> (.+?)</li>', re.UNICODE)
    if pattern.search(line):
        username = pattern.search(line).group(1)
        linenum = pattern.search(line).group(2)
        message = pattern.search(line).group(4)
        row.append(linenum)
        row.append(username)
        row.append(message)
        outfile.write(', '.join(row))
        outfile.write(u'\n')
        row = []
infile.close()
```
##### python+ BeautifulSoup
``` python
# pipi install BeautifulSoup4
from bs4 import BeautifulSoup
import io
infile = io.open('django13-sept-2014.html', 'r', encoding='utf8')
outfile = io.open('django13-sept-2014.csv', 'a+', encoding='utf8')
soup = BeautifulSoup(infile)
row = []
allLines = soup.findAll("li","le")
for line in allLines:
    username = line['rel']
    linenum = line.contents[0]['name']
    message = line.contents[3].lstrip()
    row.append(linenum)
    row.append(username)
    row.append(message)
    outfile.write(', '.join(row))
    outfile.write(u'\n')
    row = []
infile.close()
```

##### chrome_extension
- Chrome Scraper


### PDF
- 使用pdfplumber扩展包来解析PDF文档的文本和表格
- 如果只解析文本内容，也可以使用pdfminer 
- 解析英文文档内容，可以使用PyPDF2

``` python
# pip install pdfMiner

```
库名 |	说明
-----|-----
tabula|	能提取完整表格，提取结果不规范
pdfplumber|	能提取完整表格，提取结果较为规范
camelot	| 能提取完整表格和不完整表格，提取结果不规范

### RDBMS
- 关系型数据库管理系统（Relational Database Management System，RDBMS）
- Content
    - 找RDBMS中的异常数据
    - 使用不同的策略来清洗不同类型的问题数据
    - 创建新的数据表来存放清洗后的数据，包括创建子表和查找表
    - 写文档，记录数据的变化

#### Imoprt
- 观察数据
    - 避免数据导入出错
    - 把""""替换成一个"（双引号），把所有的""替换成'（单引号）
!!! attention
    - MySQL导入程序是通过引号来界定字段文本内容的
    - 多余的引号会让MySQL错误地认为这一行含有的字段个数多于实际的个数


##### scp[linux]

 参数
    scp [-12346BCpqrv] [-c cipher] [-F ssh_config] [-i identity_file]
    [-l limit] [-o ssh_option] [-P port] [-S program]
    [[user@]host1:]file1 ... [[user@]host2:]file2
    #简易版
    scp [可选参数] file_source file_target 
    
    ``` raw
    -1： 强制scp命令使用协议ssh1
    -2： 强制scp命令使用协议ssh2
    -4： 强制scp命令只使用IPv4寻址
    -6： 强制scp命令只使用IPv6寻址
    -B： 使用批处理模式（传输过程中不询问传输口令或短语）
    -C： 允许压缩。（将-C标志传递给ssh，从而打开压缩功能）
    -p： 保留原文件的修改时间，访问时间和访问权限。
    -q： 不显示传输进度条。
    -r： 递归复制整个目录。
    -v： 详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。
    -c cipher： 以cipher将数据传输进行加密，这个选项将直接传递给ssh。
    -F ssh_config： 指定一个替代的ssh配置文件，此参数直接传递给ssh。
    -i identity_file： 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。
    -l limit： 限定用户所能使用的带宽，以Kbit/s为单位。
    -o ssh_option： 如果习惯于使用ssh_config(5)中的参数传递方式，
    -P port： 注意是大写的P, port是指定数据传输用到的端口号
    -S program： 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。
    ```

##### source[MySQL]

##### load data[MySQL]
- 
``` raw
LOAD DATA
    [LOW_PRIORITY | CONCURRENT] [LOCAL]
    INFILE 'file_name'
    [REPLACE | IGNORE]
    INTO TABLE tbl_name
    [PARTITION (partition_name [, partition_name] ...)]
    [CHARACTER SET charset_name]
    [{FIELDS | COLUMNS}
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
        [ESCAPED BY 'char']
    ]
    [LINES
        [STARTING BY 'string']
        [TERMINATED BY 'string']
    ]
    [IGNORE number {LINES | ROWS}]
    [(col_name_or_user_var
        [, col_name_or_user_var] ...)]
    [SET col_name={expr | DEFAULT},
        [, col_name={expr | DEFAULT}] ...]
```

Arg| Intro
---|----
LOW_PRIORITY|使用LOW_PRIORITY修饰符，LOAD DATA语句的执行会被延迟，直到没有其他客户端从表中读取数据。这只影响仅使用表级锁的存储引擎(如MyISAM、MEMORY和MERGE)
CONCURRENT|使用CONCURRENT修饰符和满足并发插入条件的MyISAM表(也就是说，它中间不包含空闲块)，其他线程可以在执行LOAD data时从表中检索数据。即使没有其他线程同时使用这个表，这个修饰符也会略微影响LOAD DATA的性能
LOCAL|如果 load data 使用时指定了 local 关键字，则表示文件放在客户端主机上，从客户端读取文本文件；如果没指定，则表示从服务器主机读取文本文件
REPLACE|如果指定 replace ，与唯一键重复的行将被覆盖更新。对于任意记录覆盖更新时，如果唯一键外的各个字段其实都没有变化，那么执行操作时受影响行数为1；如果除唯一键外的任意字段有变化，那么执行操作时受影响行数为2
IGNORE|如果指定 ignore ，与唯一键重复的行将被忽略，默认指定 ignore
PARTITION|将数据插入指定分区
CHARACTER SET|若不指定字符集，MySQL默认使用character_set_database变量指定的字符集去读取文件，若文件字符集不同，则应指定该关键字
FIELDS TERMINATED BY|字段值的分隔符，若不指定则默认为 ‘\t’
OPTIONALLY ENCLOSED BY|字段值的包含字符，若不指定则默认为 ‘’
ESCAPED BY|字段值的转义字符，若不指定则默认为’\’
LINES TERMINATED BY|指定行分隔符，若不指定则默认为为系统的默认行分隔符（‘\r\n‘ on windows，’\n’ on linux）
LINES STARTING BY |若指定该值为xxx，则MySQL会自动去掉xxx及其前面的字符，若某行不包含xxx，则改行将被忽略，若不指定默认为’’
IGNORE LINES \| ROWS | 忽略文件开头的指定行，比如指定为2，那么MySQL只会解析并插入第三行及后面的数据

##### mysqlimport 
    -  mysqlimport命令只能指定database
    -  表名是根据文件名获取，所以文件名必须和表名一致