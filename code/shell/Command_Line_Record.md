title: 命令行（一）
date: 2022-07-13 22:30:09
categories:
- [Code,shell]
tags:
- Linux
- DataScience

toc: true
---
OSEMN——获取、清洗、探索、建模和解释数据

### 命令行 & shell
    1. shell是linux命令集的概称，是属于命令行的人机界面
    2. shell可以重复执行一些命令。也可以把要重复执行的命令写到脚本里面执行。而命令行的话就需要一个一个的输入命令，相对而言麻烦一些
<!--more-->
__区别__
    1. 直接在命令行执行，就是在当前的shell环境下执行
    2. 在脚本里执行时， 会fork一个子进程，所有操作都在子进程进行 如果涉及到一些在脚本里设置环境变量的东西，脚本结束后环境变量随即消失了

``` shell
# 用纽约时报的api
cd ~/data/NYTimes
parallel -j1 --progress --delay 0.1 --results results "curl -sL "\
"'http://api.nytimes.com/svc/search/v2/articlesearch.json?q=New+York+'"\
"'Fashion+Week&begin_date={1}0101&end_date={1}1231&page={2}&api-key='"\
"LP36jj7ilIGBLeEk" ::: {2009..2013} ::: {0..99} > /dev/null


# 1.将每500个parallel任务（或API请求）的结果合并起来
cat results/1/*/2/*/stdout | 
# 2.使用jq抽取出版日期、文档类型以及每篇文章的标题
jq -c '.response.docs[] | {date: .pub_date, type: .document_type, '\ 
# 3.使用json2csv将JSON数据转换为CSV格式，将其存为fashion.csv
'title: .headline.main }' | json2csv -p -k date,type,title > fashion.csv 

wc -l fashion.csv

< fashion.csv Rio -ge 'g + geom_freqpoly(aes(as.Date(date), color=type), '\
'binwidth=7) + scale_x_date() + labs(x="date", title="Coverage of New York'\
' Fashion Week in New York Times")' | display
```
> json2csv 需要用npm安装

### [数据科学工具箱](http://datasciencetoolbox.org/)
[下载](https://www.virtualbox.org/wiki/Downloads)

下载和安装VirtualBox、Vagrant*(虚拟机)
ls、cat、jq
``` sh
#  apt
sudo apt install path_to_deb_file
#  dpkg
sudo dpkg -i path_to_deb_file
```
五类命令行工具
工具|组成
----|---
二进制可执行文件|由源代码编译为机器代码而成
shell内置命令|由shell提供的命令行工具，如cd、help
解释型脚本|由二进制可执行文件所执行的文本文件
shell函数|由shell自己执行的函数
别名|与宏类似

常见命令行使用：
- 命令行工具grep对行进行过滤，wc计算行数，sort对行进行排序
- 最常见的将各工具组合起来方式是通过管道
` seq 100 | grep 3 | wc -l` 这里通过wc对序列为100中包含3的数字进行计数

### 输入和输出重定向
输出重定向——默认情况下，管道中最后一个命令行工具是输出到终端的，但是也可以将结果保存到文件。
``` shell
# 如果文件不存在，则自动创建；如果文件已经存在，内容将会被覆盖
seq 10 > data/ten-numbers
# 需要存储中间结果时，可以将输出保存到文件中
echo -n "Hello" > hello-world
# >> 可以将输出附加到原来内容的后面
echo " World" >> hello-world
# 很多命令行工具还允许将文件指定为命令行参数
wc -w hello-world
```

##### 常用的特殊符号
符号|说明
---|---
\#|	1.表示注释；2.命令提示符
~|表示用户主目录。切换到用户主目录下：cd ~
~+	|表示当前目录。切换到当前目录下wwwroot目录：cd ~+/wwwroot
~-	|表示上次的工作目录。切换到上次的工作目录：cd ~-
;	|在 shell 中，担任"连续指令"功能的符号就是"分号"，在命令与命令中间利用分号（；）来隔开，分号前的命令执行完成（无论成功与否）后就会立刻接着执行后面的命令。连续执行两次mkdir命令建目录：`mkdir test1;mkdir test2;`
.|	1.代表当前目录；2.文件（目录）以.开头，属于隐藏文件
''	|单引号，所见即所得，即输出单引号内容时会将单引号内的所有内容都原样输出(强引用)
""|	双引号，输出双引号内的内容时，如果内容中有命令（要反引下）、变量、转义符等，会先把变量、命令、转义字符解析出结果，然后再输出最终内容(弱引用)
\` \`	|反引号，一般用于引用命令，执行的时候命令会被执行，相当于\$()，赋值和输出都要将命令用``引起来
\	|1.转义  2. 放在命令语句的最末端，连接下一行
|	|表示管道，连结上个指令的标准输出，做为下个指令的标准输入。即将一个命令处理后得到的结果输出给下一个命令继续处理
&	|后台运行命令(守护程序)，即 & 符号放在完整指令的最后端，表示将该指令放入后台中工作。用法：命令 &。特性：关闭当前终端窗口，程序仍在运行
$|	1.变量前导符，用法： $变量，特性：调用变量，从而得到变量的值；2.普通用户的命令提示符
{}|	大括号，通常用来分离变量
()|	用括号将一串连续指令括起来，这种用法对 shell 来说，称为指令群组。例子：(cd ~ ; vcgh=pwd;echo $vcgh)，指令群组有一个特性，shell会以产生 subshell来执行这组指令。因此，在其中所定义的变量，仅作用于指令群组本身
[]	|中括号，在通配符和正则表达式中，代表一定有一个在中括号内的字符，例如：[abcd]代表一定有一个字符，且是a、b、c、d这四个任何一个，即匹配abcd中任何一个字符，abcd也可是其他任意不连续字符
[-]	|在通配符和正则表达式中都表示范围，例如：[a-z]，匹配a到z之间的任意一个字符， a到z表示范围，字符前后要连续，-表示范围的意思
[^]	|在通配符和正则表达式中都表示“非”之意如[^A-Z]，表示==非大写字符==
-	|1.表示上一次的工作目录，例如：cd -，切换到上次的工作目录中；2.系统指令的选项符号
** | 	两个星号在运算时代表 “次方” 的意思，例如：sus=2**3，表示2的3次方得数8赋值给变量sus
?	|在通配符和正则表达式中表示匹配任意一个字符，但不包含 null
*	|在通配符和正则表达式中表示匹配任意个字符
!	|表示取反、非的意思，也可以用在通配符中，例如：[!abcd]

##### 输出/输入重定向符号

符号|	说明
--|--
0	|表示标准输入（stdin），配合<或<<使用，数据流从右向左
1|	表示标准输出（stdout），配合>或>>使用，数据流从左向右
2	|标准错误（stderr），配合>或>>使用，数据流从左向右
>	|也可以写成1>，标准输出重定向，正常输出重定向到文件，会清空已有内容输出重定向，例如：命令 > file，把命令的输出重定向到文件file中。如果file已经存在，则清空原有文件，使用bash的noclobber选项可以防止复盖原有文件
<|	也可以写成0<，标准输入重定向，数据从文件流向处理的命令，例如：命令 < file，命令从file读入
<<	|也可以写成0<<，追加输入重定向，追加内容到底部，数据从文件流向处理命令
/> />	|也可以写成1>>，标准输出追加重定向，将内容追加到文件底部，不清空已有内容。例如：命令 >> file，把命令的输出重定向到文件file中，如果file已经存在，则把信息加在原有文件后面
2>	|错误输出重定向，将标准错误内容重定向到文件，如文件存在内容则清空
2>>|	错误输出追加重定向，将标准错误内容追加到文件底部，不会清空已有内容
<<<	|例如：命令 <<< word ，把word（而不是文件word）和后面的换行作为输入提供给命令

### 处理文件
文件和目录进行创建、移动、复制、重命名和删除
所有这些命令行工具都接受选项-v，意为“详细”（verbose），能使工具输出正在进行的动作。除了mkdir之外的其他所有工具都接受选项-i，意为“交互（interactive），能让工具向用户请求确认。
``` shell
# 移动（move）& 重命名
mv hello-world data
mv hello-world old-file

# 删除（remove）
rm -r ~/book/ch02/data/old

# 复制（copy）&创建备份
cp server.log server.log.bak

# 创建路径
mkdir newdirs
```
head -n 5是只显示前5行
fold将较长的行变为80字符长度
cut -c1-80则裁剪长度超过80字符的

