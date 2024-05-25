---
title: Nginx 配置反向代理去除前缀
categories:
  - 运维
tags:
  - null
date: 2024-03-30 17:07:54
---

使用Nginx做反向代理的时候如果需要根据不同的url代理到不同的服务器，需要通过以下
法：

- 地址后面加/


```
server {

      location ^~/v1/ {
          proxy_pass http://localhost:8080/;
      }
}
```

`^~/v1/`表示请求前缀是`v1`的请求，proxy_pass最后加上/，就会把`v1`去除，比如请求的地址是`v1/api/test`，则代理发出的请求是`http://localhost:8080/api/test`

- 使用`rewrite`


```
server {
    location ^~/v1/ {
        rewrite ^/v1/(.*)$ /$1 break;
        proxy_pass http://localhost:8080;
    }
}
```

使用 rewrite重写了url，注意 `proxy_pass`后不需要加/
