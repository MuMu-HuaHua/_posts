---
title: 命令行工具列表
categories: 
  - [Daily-Record,Technology]
  - shell
tags:
  - Linux
abbrlink: 62d1c985
date: 2022-07-14 22:30:09
---

部分常用工具记录

<!--more-->
1. alias
    定义或显示别名。alias是一个Bash内置命令。`alias fdfind=fd`
2. awk
   模式扫描和文本处理语言 `apt-get install mawk`
3. aws
   在命令行中管理AWS服务
4. bigmler
   `pip install bigmler`
5. body、cols、header、dseq
   body——在包含文件头的CSV文件中应用经典命令行工具
   header——添 加、 替 换 和 删 除 文 件 头 的 行
   dseq——生成一个相对于今天的日期序列
   `git clone git@github.com:jeroenjanssens/data-science-at-the-command-line.git`
   `echo -e "value\n7\n2\n5\n3" | body sort -n`
6. cowsay
   ``` shell
   apt-get install cowsay
    echo '牛爷爷给牛牛开门，牛到家了' | cowsay
    ____________________________
    < 牛爷爷给牛牛开门，牛到家了 >
    ----------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
    ```
7. csvcut、csvgrep、csvjoin、in2csv、sql2csv
    in2csv——将常见但不太好的表格数据格式转化为CSV
    `apt install csvkit`
8. curl
9. tree
    以树状格式列出目录内容 `sudo apt-get install tree`
10. type——显示命令行工具的类型。Bash内置命令
11. uniq——报告或忽略重复行 `apt-get install coreutils`
12. unzip\unrar\unpack(.sh)
13. curlicue——为curl进行OAuth协议握手操作 `git clone https://github.com/decklin/curlicue.git`
14. git
15. grep   
16. jq
17. json2csv ``    
18. less_对大文件标页数`apt install less`
19. parallel——替代for `apt-get install parallel`
20. pwd——打印当前工作目录
21. R Python
22. run_experiment
    用Python程序包scikit-learn运行机器学习实验 ` pip install skll`
23. sed——打印一个数字序列 `seq 5`
24. [Tapkee](http://tapkee.lisitsyn.m)——用各种算法对数据集降维 
    `sudo apt-get install libeigen3-dev libarpack2-dev`   
25. paste、wc、shuf、sort、spilt、tee、tail
    spilt——把一个文件切分成若干片
    sort——对文本文件的行排序
    paste——合并文件的行
    shuf——生成随即序列
    wc——对每个文件打印空行、 单词和字节个数
    tee——从标准输入中读取，并写入标准输出和文件
    `apt-get install coreutils`
    `echo 'hello world' | wc -c`
26. xml2json
    `cnpm install xml2json-command`