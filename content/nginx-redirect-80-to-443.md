---
title: nginx 80端口重定向到443端口
categories:
  - 运维
tags:
  - nginx
date: 2020-05-31 18:34:29.677000
---
使用如下配置即可


```nginx
server {
    listen 80;
    server_name www.test.com;
    rewrite ^(.*)$ https://${server_name}$1 permanent; 
}
```