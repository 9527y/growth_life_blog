---
title: nginx上传文件413错误 
categories:
  - 运维
tags:
  - nginx
date: 2020-05-28 01:02:50.488000
---
nginx报错：
**413 Request Entity Too Large**
是因为请求体积太大，解决办法：更改Nginx的配置，将客户端上传最大文件体积增大，如下配置，根据自身情况设定。

```
client_max_body_size 20M;
```

以上配置可放入`server`、`http`、`location`指令中。

# 全局配置
```
http{
	client_max_body_size 20m;
}
```
# 站点配置
修改站点下location的配置
```
    server {
        listen 80;
        server_name 75051685.xyz;
        location / {
            root   html;
            index  index.html index.htm;
        }
    }

```
修改为：
```
    server {
        listen 80;
        server_name 75051685.xyz;
        location / {
            root   html;
            index  index.html index.htm;
	    client_max_body_size 20m;
        }
    }

```