title: 命令行（一）
date: 2022-07-13 22:30:09
categories:
- [Code,shell]
tags:
- Linux
- DataScience

toc: true
---

### 命令行 & shell
    1. shell是linux命令集的概称，是属于命令行的人机界面
    2. shell可以重复执行一些命令。也可以把要重复执行的命令写到脚本里面执行。而命令行的话就需要一个一个的输入命令，相对而言麻烦一些

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
























































































































































































































































































































































