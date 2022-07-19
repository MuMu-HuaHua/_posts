---
title: Docker部署blog
categories: [Daily-Record,Technology]
tags:
  - Hexo
  - Docker
  - Linux
abbrlink: 9f9e362
date: 2022-07-18 18:30:09
---
部署记录
<!--more-->
### install
安装docker
``` sh
uname -a

# 本机
# 为了确认所下载软件包的合法性，需要添加软件源的 GPG 密钥
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 官方源
# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
# 向 sources.list 中添加 Docker 软件源
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

# 查看发行版本
apt-cache madison docker-ce 

# docker
# hexo
docker pull ubuntu  
docker run -it --name hexo ubuntu
docker run -it ubuntu
apt-get update
apt-get install -y wget
apt-get install -y git-core
cd /root
mkdir hexo
cd hexo
wget https://raw.github.com/creationix/nvm/v0.33.11/install.sh
chmod +x install.sh
./install.sh
NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node nvm install stable # 换源
npm install -g hexo-cli
npm install hexo-server --save

docker commit -m "Hexo origin submit" -a "Orange" 1ac140d51bcf hexo

# nginx

# githook
# Hook 就是在执行某个事件之前或之后进行一些其他额外的操作


# 同步
# syncpost.sh

docker run -d --name="hexo" -p 80:4000 -v /root/hexo/blog:/root/hexo/blog hexo /root/hexo/blog/start.sh

# hexo->4376(九宫格)
docker run -d \
--name="hexo" \
-p 4376:4000 \
-v /Data/chengziyu/:/root/hexo \
hexo /root/hexo/blog/start.sh



# 配置证书
docker exec -it "[your_container_name]" /bin/bash
# 申请证书
# Certbot会自动识别上文中配置的Nginx文件，加入证书验证和443端口开放。
certbot --nginx -d "[your_domain.com]"
# 手动更新
/usr/bin/certbot renew
# 自动更新
echo "0 0 1 * * /usr/bin/certbot renew --quiet" >> /etc/crontabs/root

# 配置docker ssh
mkdir ~/.ssh
echo "[your_ssh_public_key]" > ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
mkdir /run/sshd
/usr/sbin/sshd


# 本机
# hexo _config.yml
deploy:
  type: git
  repo: 
    - https://github.com/hexojs/hexojs.github.io
    - root@hexo-docker:/root/blogs.git 
    #Docker中8004端口作为SSH端口，但repo没法写端口号，写一个hexo-test作为config
  branch: master

# ssh
# ~/.ssh/config中配置你的云服务公网ip，ssh端口
Host hexo-docker
  HostName [ServerIp]
  Port 4376

```

### dockerfile
``` raw
#基于Nginx镜像
FROM nginx:latest

#换国内的源，这里用网易源
RUN sed -i 's#http://deb.debian.org#https://mirrors.163.com#g' /etc/apt/sources.list
RUN sed -i 's#http://security.debian.org#https://mirrors.163.com#g' /etc/apt/sources.list

#更新Docker中的apt-get
RUN rm -Rf /var/lib/apt/lists/* && apt-get update

#安装Git，挂载Githooks钩子，并将产物放置在var/www/hexo中
RUN apt-get install -y git
RUN git init --bare ~/blogs.git
RUN mkdir -p /var/www/hexo
RUN echo "git --work-tree=/var/www/hexo --git-dir=/root/blogs.git checkout -f" >~/blogs.git/hooks/post-receive
RUN chmod a+x ~/blogs.git/hooks/post-receive

#SSH初始化
RUN apt-get install -y openssh-server
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -y
RUN ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -y
RUN ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -y

#安装Certbot
RUN apt-get install -y certbot
RUN apt-get install -y python3-certbot-nginx

#初始化Nginx配置挂载，这里选择直接覆盖所有配置
ADD nginx/hexo.conf /etc/nginx/nginx.conf

#暴露22 SSH
#暴露80 HTTP
#暴露443 HTTPS
EXPOSE 22
EXPOSE 80
EXPOSE 443
```

在命令行操作
``` sh
docker build -t hexo-docker .
docker run -v $(pwd)/letsencrypt:/etc/letsencrypt -d -p 80:80 -p 443:443 -p 4376:22 hexo-docker
docker exec -it  hexo-docker /bin/bash
```
### 证书部署
需要进入容器
``` sh
certbot --nginx -d "[your_domain.com]"

certbot certonly --webroot -w /usr/share/nginx/html/ -d your.domain.com
# 证书更新
/usr/bin/certbot renew
# Letsencrypt 3个月需要更新一次，自动更新
echo "0 0 1 * * /usr/bin/certbot renew --quiet" >> /etc/crontabs/root
```

### shh 配置
docker容器内
``` sh
mkdir ~/.ssh
echo "[your_ssh_public_key]" > ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
mkdir /run/sshd
/usr/sbin/sshd
```

宿主机
``` sh
# 新增~/.ssh/config文件
# ~/.ssh/config中配置你的云服务公网ip，ssh端口即可
Host hexo-test
  HostName [your_remote_server_ip]
  Port 4376
```
### nginx
`nginx.conf`文件路径实在dockerfile的次级目录`/nginx/hexo.conf`
``` conf
user nginx;
worker_processes auto;

error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
  worker_connections 1024;
}

http {
  include /etc/nginx/mime.types;
  default_type application/octet-stream;

  log_format main '$remote_addr - $remote_user [$time_local] "$request" '
  '$status $body_bytes_sent "$http_referer" '
  '"$http_user_agent" "$http_x_forwarded_for"';

  access_log /var/log/nginx/access.log main;

  sendfile on;
  #tcp_nopush     on;

  keepalive_timeout 65;

  #gzip  on;

  #include /etc/nginx/conf.d/*.conf;
  server {
    listen 80;
    server_name chengziyu.xyz; 
    
    location ^~ /.well-known/acme-challenge/ {
      default_type "text/plain";
      root /var/www/hexo; 
    }

    location = /.well-known/acme-challenge/ {
      return 404;
    }


    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
      root /usr/share/nginx/html;
    }
  }
}
```

### errors
2022/07/18 04:30:12 [emerg] 107#107: location "/50x.html" is outside location
"/.well-known/acme-challenge/" in /etc/nginx/nginx.conf:39
nginx: [emerg] location "/50x.html" is outside location
"/.well-known/acme-challenge/" in /etc/nginx/nginx.conf:39
nginx: configuration file /etc/nginx/nginx.conf test failed

