---
title: Git_Record
categories: 
  - [Daily-Record,Technology]
  - git
tags:
  - Code
  - Git
abbrlink: bff17e60
date: 2022-07-12 22:30:09
---
在开始架设 Git 服务器前，需要把现有仓库导出为裸仓库——即一个不包含当前工作目录的仓库

<!--more-->
> git同步到本地
``` bash 
git clone git@github.com:shichengziya/shichengziya.github.io.git
git clone --bare chengziyu chengziyu.git

git clone root@175.178.10.108:/Data/chengziyu.git
```
> git 单向同步
``` bash
# fork 其他人的GitHub项目代码后，能单向同步更新他人代码
# 加个upstream，然后，每次用rebase更新

git remote add upstream http://github.com/xxx/test.git
git remote update upstream
git rebase upstream/master
git push origin dev:dev
```
> vscode 同步服务器
vscode连接远程服务器选定了文件夹进去，若是直接使用source control来init就会默认把整个文件夹git进去，若是只想git里面的某个子文件夹就会非常麻烦。所以选择terminal操作的方式。首先在本地准备一个文件夹。
- 先cd到要git的目录下，确定这个目录即为本地仓库，并执行。
- 将它绑定成git的原理就是在此文件夹下生成一个.git文件夹。

### 配置
``` bash 
git remote add origin git@github.com:MuMu-HuaHua/_posts.git

git add Code
git commit -m "First commit"
git push 
git push --set-upstream origin master #main
git remote -v show
git remote rm origin

error: src refspec master does not match any
git show-ref

git diff master origin/master
```
## error
ERROR: Permission to MuMu-HuaHua/_posts.git denied to duoyulee.
fatal: Could not read from remote repository.

Please make sure you have the correct access rightsand the repository exists.

``` shell
#查看全局配置 主要看user.name user.email user.password
git config --list

#移除全局配置账户
git config --global --unset user.name
#查看全局用户名 已经显示为空了，说明移除成功
git config --global user.name
# 移除全局配置邮箱
git config --global --unset user.email
# 再查看全局邮箱 已经显示为空了，说明移除成功
git config --global user.email
# 移除全局密码
git config --global --unset user.password
# 查看全局密码 已经显示为空了，说明移除成功
git config --global user.password


# 3.设置git用户名、密码、邮箱的配置
$ git config user.name "freedom"
$ git config user.password "123456"
$ git config user.email "1548429568@qq.com"
# 3.设置git用户名、密码、邮箱的配置（全局配置）
$ git config --global user.name 用户命
$ git config --global user.name freedom
$ git config --global user.password 密码
$ git config --global user.password abc0506abc
$ git config --global user.password 邮箱
$ git config --global user.email " "
 
 
# 4.修改git用户名、密码、邮箱的配置
$ git config user.name "freedom"
# 4.修改git用户名、密码、邮箱的配置（全局配置）
$ git config --global user.name "freedom"
 
 
# 5.删除git用户名、密码、邮箱的配置
$ git config --global --unset user.name "yourName"
$ git config --global --unset user.email "your@email.com"
```
公钥
私钥
全局（用户名、密码、邮箱）

kex_exchange_identification: Connection closed by remote host


Hi duoyulee! You've successfully authenticated, but GitHub does not provide shell access.

### 记录
1. 记录冲突
   方法一：stash
   `git stash`
   `git pull`
   `git stash pop`
   方法二：直接完全覆盖本地修改
   `git reset --hard`
   `git pull`















