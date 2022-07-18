title: 命令行工具
date: 2022-07-09 18:30:09
category:
- [Daily-Record,Technology]
tags:
- Linux

toc: true
---
常用工具的替代
<!--more-->

### du的替代：ncdu
分析磁盘，然后按照最常用的顺序显示目录或文件

可以通过方向键导航到每个结果。如果在选中的结果上按下Enter，则ncdu将显示该目录的内容：

### top的替代：htop
htop是一个类似于top的交互式进程浏览器，提供了更好的用户体验。在默认情况下，htop显示的各项指标与top相同

### man的替代：tldr

tldr命令行工具显示可以简化的命令文档。实际上这个工具不是man的替代品。man pages仍然是许多工具的规范以及完整的信息源。
tldr工具需要访问互联网才能查询tldr页面

### sed/grep查找JSON数据的替代：jq



### find的替代：fd
fd是find命令的一种简单快速的替代。它的目的不是替换find的功能，而是提供一些合理的默认值，在某些情况下非常有用。

在默认情况下，fd会针对当前目录执行不区分大小写的模式搜索，并输出彩色的结果。使用find进行的相同搜索时，需要提供其他命令行参数。例如，搜索当前目录中所有的markdown文件（即.md或.MD文件），
``` sh
find . -iname "*.md" 

# apt-get install fd-find
# 设置别名 
alias fd=fdfind

# fdfind -e md
fdfind .md
# 在特定路径查找含该文本的文件
fd filename /dir

fdfind --version

#查找文件类型为md的文件
# man td
# 查看具体参数含义
fd -tf md
```