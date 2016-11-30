<!--
author: jibo
date: 2016-11-14
title: edoctor 调试部署情况
tags: docker, edoctor
category: Notes
status: publish
summary:
-->

> 调试期间，服务器部署情况如下：

# 路径 #
```
/root/edoctor-web
/root/edoctor-admin
/root/edoctor-doctor
/root/edoctor-hospital
/root/edoctor-partner
/root/edoctor-api
```

# 前端 #
> Dockerfile of edoctor-web:latest (only used for building)
> From node:6.9.1
> Volume /data/
> WORKDIR /data/

#### Build ####
```
docker run --rm -it -v /root/edoctor-web:/data edoctor-web npm i
docker run --rm -it -v /root/edoctor-web:/data edoctor-web npm run build

docker run --rm -it -v /root/edoctor-doctor:/data edoctor-web npm i
docker run --rm -it -v /root/edoctor-doctor:/data edoctor-web npm run build

docker run --rm -it -v /root/edoctor-admin:/data edoctor-web npm i
docker run --rm -it -v /root/edoctor-admin:/data edoctor-web npm run build

docker run --rm -it -v /root/edoctor-hospital:/data edoctor-web npm i
docker run --rm -it -v /root/edoctor-hospital:/data edoctor-web npm run build

docker run --rm -it -v /root/edoctor-partner:/data edoctor-web npm i
docker run --rm -it -v /root/edoctor-partner:/data edoctor-web npm run build
```

#### Startup ####
```
docker run --name edoctor --restart=always -v /root/edoctor-web:/root/edoctor-web -v /root/edoctor-doctor:/root/edoctor-doctor -v /root/edoctor-hospital:/root/edoctor-hospital -v /root/edoctor-partner:/root/edoctor-partner -v /root/edoctor-admin:/root/edoctor-admin -v /root/nginx/conf:/etc/nginx/conf.d -v /root/nginx/log:/var/log/nginx -v /root/nginx/html:/usr/share/nginx/html -p 80:80 -d nginx:edoctor-web
```

#### Config ####
> 每次新建容器需要登录容器修改一下权限；待产品稳定后可以考虑调整写入 Dockerfile
```
docker exec edoctor chown -R nginx:nginx /root
```

# 后端 #
```
docker run -d --restart=always --name memcache memcached:1.4.33-alpine
docker run --name edoctor-api --restart=always -p 81:80 --link memcache:my_memcache -v /root/edoctor-api:/var/www/html -d php:edoctor-api
```
