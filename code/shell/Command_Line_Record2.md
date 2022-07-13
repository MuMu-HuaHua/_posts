title: 命令行（二）
date: 2022-07-13 14:30:09
categories:
- [Code,shell]
tags:
- Linux
- DataScience

toc: true
---

常用的获取数据工具： curl、sql2csv和tar

### 解压缩
tar
zip
rar
``` sh
#!/usr/bin/env bash
# unpack: Extract common file formats
# Display usage if no parameters given
  if [[ -z "$@" ]]; then
    echo " ${0##*/} <archive> - extract common file formats)"
    exit
  fi
# Required program(s)
req_progs=(7z unrar unzip)
for p in ${req_progs[@]}; do
  hash "$p" 2>&- || \
  { echo >&2 " Required program \"$p\" not installed."; exit 1; }
done
  # Test if file exists
  if [ ! -f "$@" ]; then
    echo "File "$@" doesn't exist"
  exit
  fi
  # Extract file by using extension as reference
  case "$@" in
  *.7z ) 7z x "$@" ;;
  *.tar.bz2 ) tar xvjf "$@" ;;
  *.bz2 ) bunzip2 "$@" ;;
  *.deb ) ar vx "$@" ;;
  *.tar.gz ) tar xvf "$@" ;;
  *.gz ) gunzip "$@" ;;
  *.tar ) tar xvf "$@" ;;
  *.tbz2 ) tar xvjf "$@" ;;
  *.tar.xz ) tar xvf "$@" ;;
  *.tgz ) tar xvzf "$@" ;;
  *.rar ) unrar x "$@" ;;
  *.zip ) unzip "$@" ;;
  *.Z ) uncompress "$@" ;;
  * ) echo " Unsupported file format" ;;
esac
```
创建脚本后的简单使用——`unpack logs.tar.gz`

### 格式转换

#### Excel
csv文件格式定义
- 每条记录占据独立一行，由换行符（CRLF）分隔开
- 文件中的最后一条记录有无末尾换行符皆可
- 在文件的首行可以有标题行（可选的），其格式与普通记录行相同
  - 标题行要包含文件中对应字段的名字，字段数量还应与文件中其他记录相同
  - 是否存在标题行应由该文件的MIME
类型的可选的header参数来指定）。
in2csv、csvcut和csvlook是csvkit中的一个常用工具，
``` shell
sudo apt install csvkit

# 转换数据
in2csv data/imdb-250.xlsx > data/imdb-250.csv

# 查看数据  csvlook（生成表格形式的数据视图）
in2csv data/imdb-250.xlsx | head | csvcut -c Title,Year,Rating | csvlook
```

#### sql
sql2csv也是csvkit的一个工具,可以在许多不同的数据库上执行查询，包括MySQL、Oracle、PostgreSQL、SQLite、微软SQL Server和Sybase。sql2csv也支持INSERT、UPDATE和DELETE查询

``` shell
sql2csv --db 'sqlite:///data/iris.db' --query 'SELECT * FROM iris '\
  'WHERE sepal_length > 7.5'
# --db 的参数
# dialect+driver://username:password@host:port/database
```



### Download
``` sh
# 下载——curl
# 从古腾堡计划（Project Gutenberg）中下载马克·吐温的《哈克贝利·费恩历险记》
# 指定-s（代表silent）选项，这样就不会显示进度条
curl -s http://www.gutenberg.org/cache/epub/76/pg76.txt | head -n 10
# 用-o选项明确地指定输出文件来保存数据
curl -s http://www.gutenberg.org/cache/epub/76/pg76.txt -o data/finn.txt
# 当URL有密码保护时，可以指定用户名和密码
curl -u username:password ftp://host/file
# 访问简写的网址指定-L或--location选项来重定向，避免仅返回html
curl -L j.mp/locatbbar
# 指定-I或--head选项，curl就只获取响应信息的HTTP头部数据
curl -I baidu.com

# 访问api
# sudo apt install jq
curl -s http://api.randomuser.me | jq '.'
```


### Shell-Script
创建流程：
1. 复制并粘贴单行到文件中
2. 添加执行权限
   1. chmod
   2. u+x选项包括三个特性：(1)u表明想要改变文件拥有者（就是你）的权限，因为是你创建了该文件；(2)+表明想要添加权限；(3)x表明是执行权限
   3. `-rwxrw-r--`
      1. 第一个字符，==-==，表明文件类型。-意为常规文件，d（这里没有出现）意为目录。
      2. 随后的三个字符，rwx，表明文件拥有者的访问权限。r和w分别代表读和写。（如果-替换了x，意思是不能执行该文件。）
      3. 后面的三个字符，rw-，表明拥有该文件的用户组中所有成员的访问权限。
      4. 最后，第一列的最后三个字符，r--，表明其他所有用户的访问权限。
3. 定义==shebang==
   1. shebang是脚本中的特殊行，它告诉系统用哪个可执行文件来解释命令
   2. shebang这个名字源于该行的前两个字符：井号#（she）和惊叹号!（bang）
   3. 指定bash执行脚本 `#!/usr/bin/env bash`
   4. env命令行工具能够获知bash和python的安装目录
4. 删除固定的输入部分。
5. 参数化
   1. ==$1==是Bash中的一个特殊值
   2. 存放的是传递给命令行工具的第一个命令行参数的值
6. 视情况扩展PATH
   1. `echo $PATH | fold`
   2. 要永久修改PATH，需要编辑主目录中的.bashrc或.profile文件

#### Python & R 
1. 命令行工具并不需要扩展名。实际上，命令行工具很少有扩展名
1. 在命令行中，==!!会被替换为刚才运行的命令==


##### 流数据处理
Python 示例
``` bash
#!/usr/bin/env python
from sys import stdin, stdout
while True:
  line = stdin.readline()
  if not line:
      break
  stdout.write("%d\n" % int(line)**2)
  stdout.flush()
```

R 示例
``` bash
#!/usr/bin/env Rscript
f - file("stdin")="" open(f)="" while(length(line="">->- readlines(f,="" n="1))"> 0) {
        write(as.integer(line)^2, stdout())
}
close(f)
->
```







































































