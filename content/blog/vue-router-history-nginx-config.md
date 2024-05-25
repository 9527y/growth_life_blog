---
title: vue-router的history模式nginx配置
categories:
  - 运维
tags:
  - nginx
date: 2021-01-06 17:15:12.464000
---
原理是：将解析转为index.html页面

```
server {
        listen 80;
        server_name avue-data.meipinshu.cn;

        location / {
                index index.html;
                root /data/wwwroot;
                try_files $uri $uri/ /index.html;

        }
}
```
