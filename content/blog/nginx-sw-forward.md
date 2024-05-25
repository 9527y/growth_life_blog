---
title: Nginx转发websocket协议
categories:
  - 运维
tags:
  - nginx
  - websocket
date: 2022-01-07 17:03:17
---

## Nginx转发

Nginx添加WebSocket的转发配置。
```
location /websocket/ {
        proxy_pass http://myserver;
 
        proxy_http_version 1.1;
        proxy_read_timeout 360s;   
        proxy_redirect off;   
        proxy_set_header Upgrade $http_upgrade; 
        proxy_set_header Connection "upgrade";    #配置连接为升级连接
        proxy_set_header Host $host:$server_port;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```
使用如上连接，如果所有的连接仅仅为 "ws" 协议的请求是没有问题的，但是如果要及支持 http 请求又支持 ws 请求上述配置就不起作用了。

## 既支持http又支持 ws 的配置。

```
http {
 
   #自定义变量 $connection_upgrade
    map $http_upgrade $connection_upgrade { 
        default          keep-alive;  #默认为keep-alive 可以支持 一般http请求
        'websocket'      upgrade;     #如果为websocket 则为 upgrade 可升级的。
    }
 
    server {
        ...
 
        location /chat/ {
            proxy_pass http://backend;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade; #此处配置 上面定义的变量
            proxy_set_header Connection $connection_upgrade;
        }
```