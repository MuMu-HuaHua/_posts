---
title: 端口相关
categories: [Daily-Record,Technology]
tags:
  - nginx
  - Linux
abbrlink: 8fca1d44
date: 2022-07-22 18:30:09
---
端口相关记录
<!--more-->

### nginx部署证书踩坑
#### 443端口显示正被docker监听
`lsof -i:443`
`ss -nplut |grep 443`

#### 无法连接443端口



### 域名
查看域名dns
`nslookup -type=NS chengziyu.xyz`
`nslookup -type=TXT   _acme-challenge.chengziyu.xyz`
[查看域名申请证书记录](https://crt.sh/)

certbot --manual --preferred-challenges dns certonly

nslookup -type=TXT  _aksldja21i1.<domain> <name server>