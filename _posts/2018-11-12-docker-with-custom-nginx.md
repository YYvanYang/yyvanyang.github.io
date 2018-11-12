---
layout: post
title:  "自定義nginx docker鏡像"
---

## NGINX作为WebSocket代理

该网页套接字协议提供创建支持客户端和服务器之间的实时双向通信的Web应用程序的方式。作为HTML5的一部分，WebSocket使开发这些类型的应用程序比以前可用的方法更容易。大多数现代浏览器都支持WebSocket，包括Chrome，Firefox，Internet Explorer，Opera和Safari，现在越来越多的服务器应用程序框架也支持WebSocket。

对于企业生产用途，需要多个WebSocket服务器来提高性能和高可用性，需要一个理解WebSocket协议的负载均衡层，NGINX从1.3版开始就支持WebSocket，可以作为反向代理并对WebSocket 进行负载均衡应用。（所有版本的NGINX Plus也支持WebSocket。）

查看有关NGINX可扩展性的最新性能测试，以平衡WebSocket连接。

WebSocket协议与HTTP协议不同，但WebSocket握手与HTTP兼容，使用HTTP Upgrade工具将连接从HTTP升级到WebSocket。这使WebSocket应用程序更容易适应现有的基础架构。例如，WebSocket应用程序可以使用标准HTTP端口80和443，从而允许使用现有的防火墙规则。

WebSocket应用程序在客户端和服务器之间保持长时间运行的连接，从而促进实时应用程序的开发。用于将连接从HTTP升级到WebSocket的HTTP升级机制使用Upgrade和Connection标头。逆向代理服务器在支持WebSocket时面临一些挑战。一个是WebSocket是逐跳协议，因此当代理服务器拦截来自客户端的升级请求时，它需要将自己的升级请求发送到后端服务器，包括适当的头。此外，由于WebSocket连接是长期存在的，与HTTP使用的典型短期连接相反，反向代理需要允许这些连接保持打开，而不是关闭它们，因为它们似乎是空闲的。

NGINX支持WebSocket，允许在客户端和后端服务器之间建立隧道。要使NGINX将升级请求从客户端发送到后端服务器，必须明确设置Upgrade和Connection标头，如下例所示：

```
location /wsapp/ {
    proxy_pass http://wsbackend;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
}
```

完成此操作后，NGINX将此作为WebSocket连接处理。

```
http {
    map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
    }

    upstream websocket {
        server 192.168.100.10:8010;
    }

    server {
        listen 8020;
        location / {
            proxy_pass http://websocket;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
        }
    }
}
```

让NGINX正确处理WebSocket所需的只是正确设置标头以处理升级请求，升级请求将连接从HTTP升级到WebSocket。

## 自定義NGINX Docker基礎鏡像

由於/etc/nginx/conf.d/default.conf文件下無法配置nginx的http模塊，需要自定義一個基礎的nginx docker礎鏡像來配置http模塊。

基礎鏡像配置文件如下：
/etc/nginx/nginx.conf

```

#user  nobody;
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    # enable web sockets protocol
    map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
    }

    # enable compression
    gzip on;
    gzip_http_version 1.0;
    gzip_proxied any;
    gzip_min_length 256;
    gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/vnd.ms-fontobject application/x-font-ttf font/opentype image/svg+xml image/x-icon;

    sendfile        on;
    keepalive_timeout  65;
    # 注意這裏，需要包含自定義配置文件，這是特定nginx鏡像的配置文件。
    include /etc/nginx/conf.d/*.conf;
}

```

nginx.dockerfile

```
FROM nginx:1.14.0-alpine
COPY ./nginx.conf /etc/nginx/nginx.conf
```

## 靜態文件服務器docker nginx server

nginx-custom.conf

```
server {
  listen 80;
  location / {
    root /usr/share/nginx/html;
    index index.html index.htm;
    try_files $uri $uri/ /index.html =404;
  }

  location /api {
    rewrite ^/api/?(.*) /$1 break;
    proxy_pass http://192.168.1.173:10020;
    proxy_read_timeout 300s;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    # proxy_http_version 1.1;
    # proxy_set_header Upgrade $http_upgrade;
    # proxy_set_header Connection $connection_upgrade;
  }


  # websocket connect
  location /capture {
    proxy_pass http://192.168.1.173:10020;

    keepalive_timeout 0;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
  }


}


```

Dockerfile

```
#
# ---- build scripts ----
FROM node:10.13.0-alpine AS build

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

# install ALL node_modules, including 'devDependencies'
RUN npm config set registry https://registry.npm.taobao.org -g
RUN npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/ -g
RUN npm install

# Bundle app source
COPY . .

RUN npm run build

#
# ---- Release ----
# FROM nginx:1.14.0-alpine AS release
FROM nginx-base AS release

LABEL maintainer="yvan yang"

COPY --from=build /usr/src/app/build /usr/share/nginx/html
COPY ./nginx-custom.conf /etc/nginx/conf.d/default.conf

ENV LANG='C.UTF-8' LC_ALL='C.UTF-8' TZ='Asia/Shanghai'

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone


```

### Docker build腳本

linux平臺，image_build.sh
```
#!/bin/bash

# build nginx base image
if [ ! -z $(docker images -q nginx-base) ]; then
  echo "Using existing nginx-base image!"
else
  cd nginx
  echo "Building nginx-base image..."
  docker build -t nginx-base -f nginx.dockerfile .
  cd ..
fi

echo "Building yvan.com/fed:1.0.0 image..."
docker build -t yvan.com/fed:1.0.0 .

```

window平臺，image_build.ps1
```
$ErrorActionPreference = "Stop"

# build image
if(docker images -q nginx-base){
    # "using existing nginx-base image"
}else{
    cd nginx
    # "Building nginx-base image..."
    docker build -t nginx-base -f nginx.dockerfile .
    cd ..

}

# "Building yvan.com/fed:1.0.0 image..."
docker build -t yvan.com/fed:1.0.0 .
```

##### 參考

- https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/
- https://www.nginx.com/blog/websocket-nginx/
