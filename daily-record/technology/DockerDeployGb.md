---
title: Docker部署Gitbook
categories: [Daily-Record,Technology]
tags:
  - Gitbook
  - Docker
  - Linux
date: 2022-07-22 18:30:09
---
<!--more-->
### docker部分

### 宿主机
``` sh
docker build . -t gb-docker

docker run -d \
-p 2722:4000 \
-v $(pwd)/gitbook:/gitbook  gb-docker gitbook init

docker run \
--name gb-docker \
-v $(pwd)/letsencrypt:/etc/letsencrypt -d \
-v $(pwd)/gitbook:/gitbook -d \
-p 88:80 -p 444:443 \
-p 2722:4000 gb-docker

docker run -d  \
--name gb-docker \
-v $(pwd)/letsencrypt:/etc/letsencrypt \
-v $(pwd)/gitbook:/gitbook  \
-p 1080:80 -p 1443:443 \
-p 2722:4000 gb-docker  gitbook serve

docker run -dit \
--name gb-docker \
-v $(pwd)/letsencrypt:/etc/letsencrypt \
-v $(pwd)/gitbook:/gitbook  \
-p 2722:4000 gb-docker  gitbook serve
```

##### 重新构建
每次改动md源文件后，都要重新构建，命令：
docker exec gitbook gitbook build . /srv/html


docker run -d  \
--name gb-docker \
-v $(pwd)/gitbook:/gitbook  \
-p 1080:80 -p 1443:443 \
-p 2722:4000 gb-docker  gitbook serve