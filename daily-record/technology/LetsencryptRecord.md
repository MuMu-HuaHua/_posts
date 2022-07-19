---
title: Certbot证书相关记录
categories: [Daily-Record,Technology]
tags:
  - Linux
abbrlink: 741cac61
date: 2022-07-22 18:30:09
---
### 账户
账户信息都保存在/etc/letsencrypt/account目录下，查看目录结构：
`sudo tree /etc/letsencrypt·



### dns认证
[认证原理](https://letsencrypt.org/zh-cn/how-it-works/)
``` sh
# 生成一段认证字符串
# 需要到相应dns控制台添加txt解析记录
# 主机名为_acme-challenge
certbot --manual --preferred-challenges dns certonly

# 查看txt解析是否解析完毕
dig -t txt _acme-challenge.example.com
# nslookup -type=TXT  _acme-challenge.example.com
certbot certonly --webroot -w /var/www/example -d www.example.com -d example.com -w /var/www/other -d other.example.net -d another.other.example.net
```





