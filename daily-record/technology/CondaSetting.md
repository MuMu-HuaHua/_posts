---
title: 命令行工具
categories: [Daily-Record,Technology]
tags:
  - Linux
abbrlink: 6d2a527e
date: 2022-07-17 18:30:09
---
conda & jupyter 配置
<!--more-->
### conda install
[清华镜像源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)
```
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2021.11-Linux-x86_64.sh
bash Anaconda3-2021.11-Linux-x86_64.sh
# 卸载
# rm -rf ~/anaconda3 ~/.condarc ~/.conda ~/.continuum
# 需要刷新环境变量
source ~/.bashrc
```
### jupyter setting
配置jupyter：后台、端口、路径
``` sh
jupyter notebook --generate-config
# Writing default config to: /root/.jupyter/jupyter_notebook_config.py
python
from notebook.auth import passwd
pwd()
# 输入密码后复制输出的密钥，用于下一步的文件配置

vim /root/.jupyter/jupyter_notebook_config.py
# c.NotebookApp.allow_remote_access = True #允许远程连接
# c.NotebookApp.ip='*' # 设置所有ip皆可访问
# c.NotebookApp.password = '' #之前复制的密码
# c.NotebookApp.open_browser = False # 禁止自动打开浏览器
# c.NotebookApp.port =8888 #任意指定一个端口
# c.NotebookApp.default_url = 'path'

# 不显示token，开启notebook之后查看token
jupyter notebook list

# 后台
# ​nohup​​ 表示no hang up，不挂起，命令执行后即使终端退出，服务也不会停止。
# 指定日志文件路径为​​/jupyter/jupyter.log​​。
jupyter notebook --allow-root \
nohup jupyter notebook >~/jupyter.log 2>&1 &

# 改密码
jupyter notebook password

# 一步到位
# nohup jupyter notebook --allow-root > jupyter.log 2>&1 &
```



