---
title: redis 5.x安装
categories:
  - 运维
tags:
  - redis
date: 2020-07-31 14:57:13.226000
---
下载地址：
```url
https://redis.io/download
```

安装环境：
```shell
yum install -y gcc
```

编译安装
```
make distclean  && make
```
修改配置
```
vi redis.conf
```
运行
```
src/redis-server redis.conf
```

