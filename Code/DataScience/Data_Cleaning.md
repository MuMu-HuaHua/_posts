title: 数据清洗(1)
date: 2022-07-7 22:30:09
categorie:
- [Code,DataScience]
- [Statistic,Theory]

tags: 
- 数据清洗
- SQL
- Linux
- Python
---

## 前置 
###  流程
1.  分析问题
1.  数据收集
1.  数据清洗
1.  数据分析&机器学习
1.  可视化实现
1.  问题决议

记得保留日志，时间长了很可能会忘记对数据操作的流程

<!--more-->

### 格式、类型、编码
#### 格式
1. 文本文件与二进制文件
1. 常见拓展名
    1. 以.xlsx为扩展名的Excel文件
    1. 以.docx为扩展名的Word文件
    1. 以.pptx为扩展名的Powerpoint文件；
    1. 以.png、.jpg和.gif为扩展名的图形文件；
    1. 以.mp3、.ogg、.wmv和.mp4为扩展名的音乐和视频文件；
    1. 以.txt为扩展名的文本文件。
1. 文本文件类型
    1. 分隔格式（结构化数据）
        1. 制表符分隔值（TSV）
        1. 逗号分隔值（CSV）
    1. JSON格式（半结构化数据）
    1. HTML格式（非结构化数据）

查看不可见字符
    CSV、TSV的换行符和回车符都是看不见的
    查看方式
        1. Mac系统中使用Text Wrangler
        1. Windows上的Notepad++
        1. Linux系统或Mac上的Terminal窗口中使用vi
            1. 使用vim打开文件
            1. `set list` + enter
            1. vim 中不可见字符会显示为$


1. 分隔文件创建者需要在整个数据表创建之前，删除最后一列中的逗号（也就是说被分割的数据中不可以包含逗号字符）
2. 分隔文件创建者需要使用额外的符号来对数据进行封闭处理
    1. 通常的做法是采用双引号来对数据进行封闭
    2. 比如某一行末端出现的129,000会被改成"129,000"

字符转义
    如果数据本身就含有引号,就需要使用另外一个特殊字符来对数据内部使用的引号进行转义，而转义字符就是反斜线   \

1. JSON格式(半结构化数据)
    1. 半结构化数据集的特点是数据的值都有其对应的属性标识，而且顺序无关紧要
    2. 字符串值必须使用==双引号==进行封闭处理
    3. 字符串内部的双引号也都必须用反斜线进行转义
    4. 如果转义字符（\）被当作数据内容使用，它本身也必须被转义
    ``` json
    {
        "firstName": "Sally",
        "birthDate": "1971-09-16",
        "faveColor": "light\"Carolina\" blue",
        "salary":129000
    }
    ```

#### 归档与压缩

1. tar（磁带归档）
    ``` shell
    tar cvf fileArchive.tar reallyBigFile.csv anotherBigFile.csv
    tar xvf fileArchive.tar
    ```
1. 压缩
    1. 有gui的压缩软件
    1. tar自带的gzip和bzip2
        1. 只能一次压缩一个文件（或一个归档文件）
        1. tar的工作是把多个文件整合到一个独立的文件
        1. gzip
            压缩一个新创建的.tar文件，可以在tar命令的后面添加一个z选项
            ``` shell
            tar cvzf fileArchive.tar.gz reallyBigFile.csv anotherBigFile.csv

            //或者分两步
            tar cvf fileArchive.tar reallyBigFile.csv anotherBigFile.csv
            gzip fileArchive.tar

            //解压
            gunzip fileArchive.tar.gz
            tar xvf fileArchive.tar
            ```
            1. bzip2
            ``` shell
            tar cvzf fileArchive.tar.gz reallyBigFile.csv anotherBigFile.csv

            //或者分两步
            tar cvjf fileArchive.tar.bz2 reallyBigFile.csv anotherBigFile.csv

            //解压
            bunzip2 fileArchive.tar.bz
            tar xvf fileArchive.tar
            ```


默认情况下，大部分压缩程序和归档程序都会删除原始文件

gzip压缩和解压缩都比较快，并且在每个装有OS X或Linux的机器上都可以使用

RAR是Windows上广泛使用的归档和压缩解决方案；但是在OS X和Linux系统上只有特殊的软件才能处理这种格式

---

#### 数据类型、空值与编码
1. 数字类型数据
    1. 整数
    1. 小数
1. 日期和时间
1. 字符串
1. 其他数据类型
    1. 集合/枚举
    1. 布尔
    1. Blob（二进制大对象）
1. 类型互换
    1. 风险
        1. 同类型之间的不同范围
        1. 不同精度的转换
    1. 转换策略
        1. 基于SQL的操作。这种策略适用于处理保存在数据库中的数据
            1. 把concat()和日期时间函数结合起来使用
            ``` SQL
            SELECT concat(hour(date),
            ':',
            minute(date),
            # 必须要包含a.m/p.m，可以使用条件语句来判断小时部分的值
            if(hour(date)<12,'am','pm'),
            contact(
                ', ',
                dayname(date),
                ', ',
                monthname(date),
                ' ',
                day(date),
                ', ',
                year(date))
            )
            FROM message
                WHERE mid=52;
            ```
            1. 使用MySQL提供的更为高级的date_format()函数
            ``` SQL
            SELECT concat(
                date_format(date, '%l:%i'),
            # 如果需要am/pm小写的话需要指定
                lower(date_format(date,'%p ')),
                date_format(date,'%W, %M %e, %Y')
            )
            FROM message
                WHERE mid=52;
            ``` 
            1. 从字符串类型转换到MySQL的日期类型
                1. str_to_date()
                ``` SQL 
                # 从第79条记录所引用的电子邮件内容中提取时间
                SELECT
                str_to_date(
                    substring_index(
                        substring_index(reference,'>',3),
                        'Sent: ',
                        -1
                    ),
                    '%W,%M %e, %Y %h:%i %p'
                )
                FROM referenceinfo
                WHERE mid=79;
                ```

            1. 把MySQL字符串类型的数据转换成小数
                ``` SQL
                # convert()与cast()的功能相似。把某种数据类型转成数字的功能
                # March had slipped by 51 cts at the same time to trade at $18.47/bbl.
                SELECT convert(
                        substring_index(
                            substring(
                                body,
                                locate('$',body)+1
                            ),
                            '/bbl',
                            1
                        ),
                        decimal(4,2)
                    ) as price
                FROM message
                WHERE body LIKE "%$%" AND body LIKE "%/bbl%" AND sender ='energybulletin@platts.com';
                ```     
        1. 基于文件的操作。这种策略可以用来处理文本类型的数据文件
            1. 最常见的是电子表格(EXCEL)或JSON文件
            1. 需要把从文件中读取出来的数据再次以某种方法进行进一步加工
                1. PHP代码所生成的JSON数据都是直接来自数据库的
    1. 空值处理
        1. 空值类型
            1. NULL: NULL不等于任何值，甚至是它本身
                1. 只有在不希望出现任何数据的情况下才应该使用NULL
                1. 要区分没有数据和不清楚是否有数据
            1. 0 : 零是可测量的数字，在数值系统中是有意义的
            1. 空值: 表现空字符串的正确方式就是把它“留空”
                1. " "（两个双引号中间夹着一个空格字符，有时候也称空白，但空格的说法可能更为恰当一些）不等同于""
                1. JSON有些时候也是可能出现空对象和空字符串的
                1. 除了空格外，有时其他不可见字符也会被误当成空或空白来解析(制表符、回车、换行符) 
    1. 编码格式转换

不同的数据环境和编程环境（DBMS、存储系统和编程语言）在对待零、空值和NULL这三个概念时稍有不同

#### 数据转换

##### 基于工具
- 少量或中量数据的转换
- Excel转csv（另存为）
    - 表格中有多个工作表的话，需要分别把每一个工作表单独保存为CSV文件
- 表格转json
##### 基于编程
- python
    - CSV到JSON
        - csv + json
        - csvkit
    ``` python
    import json
    import csv
    with open("filename.csv" as f):
        file_csv = csv.DictReader(file)
        output = '['
        # 处理每一个目录
        for row in file_csv:
            output += json.dumps(row) + ","
        output = output.rstrip(',')  + "]"
    # 保存文件
    file = open("jsonfile.json",'w')
    f write(output)
    f.close()

    # pip install csvkit
    import csvkit
    csvjson csvfile.csv > jsonfile.json
    # csvkit 中还有csvcut（从CSV文件中提取任意指定字段中的数据）
    # csvformat（用于改变CSV文件中的分隔符或换行符）
    # csvkit中的命令行工具都支持以重定向的方式创建新文件
    # 提取1，3行数据创建新数据
    # csvcut file1.csv -c 1,3 > file2.csv
    ```
    - json 到 csv
    ``` python
    import json , csv
    with open("csvfile.csv","r") as f:
        dicts = json.load(f)
    out = open("csvfile.csv",'w')
    writer = csv.DictWriter(out, dicts[0].keys())
    writer.writerheader()
    writer.writerrows(dicts)
    out.close()
    ```

    - SQL到JSON
        - 连接数据库
        - SQL_CURD
        - 将结果转换为JSON

    - SQL到CSV
        - 连接数据库
        - SQL_CURD
        - 将结果转换为JSON
        




##### Project1
``` python
import urllib2
import time
with open('GGurls.txt', 'r') as f:
    urls = []
    for url in f:
        urls.append(url.strip())

currentFileNum = 1
for url in urls:
    print("Downloading: {0} Number: {1}".format(url, currentFileNum))
    time.sleep(2)
    htmlFile = urllib2.urlopen(url)
    urlFile = open("msg%d.txt" %currentFileNum,'wb')
    urlFile.write(htmlFile.read())
    urlFile.close()
    currentFileNum = currentFileNum +1

# 从RSS中提取URL，收集并解析HTML
import re
import urllib2
import datetime
import numpy
alllinks = []
timelist = []
for filename in os.listdir(os.getcwd()):
    if filename.endswith('.rss'):
        f = open(filename, 'r')
        linktext = ''
        linkurl = ''
        for line in f:
        # 找出讨论主题的的URL
            linktext = re.search('(<guid>)(.+?)(<\/guid>)', line)
            if linktext:
                linkurl= linktext.group(2)
                alllinks.append(linkurl)
        f.close()
mainmessage = ''
reply = ''
maindateobj = datetime.datetime.today()
replydateobj = datetime.datetime.today()
for item in alllinks:
    print("===")
    print("working on thread\n" + item)
    response = urllib2.urlopen(item)
    html = response.read()
    # 定义一个用于匹配时间戳的正则表达式样式
    tuples = re.findall('lia-message-posted-on\">\s+<spanclass=\"local-date\">\\xe2\\x80\\x8e(.*?)<\/span>\s+<spanclass=\"local-time\">([\w:\sAM|PM]+)<\/span>', html)
    mainmessage = tuples[0]
    if len(tuples) > 1:
        reply = tuples[1]
    if mainmessage:
        print("main: ")
    maindateasstr = mainmessage[0] + " " + mainmessage[1]
    print maindateasstr
    maindateobj = datetime.datetime.strptime(maindateasstr,'%m-%d-%Y %I:%M %p')
    if reply:
        print("reply: ")
        replydateasstr = reply[0] + " " + reply[1]
        print(replydateasstr)
        replydateobj = datetime.datetime.strptime(replydateasstr,'%m-%d-%Y %I:%M %p')
# 只针对有回复的数据进行时间差计算
        difference = replydateobj - maindateobj
        totalseconds = difference.total_seconds()
        timeinhours = (difference.days*86400+difference.seconds)/3600
    if timeinhours > 1:
        print(timeinhours)
        timelist.append(timeinhours)
print("when all is said and done, in hours:") 
print(numpy.mean(timelist)) 
print(numpy.std(timelist))
print(numpy.median(timelist))
```
