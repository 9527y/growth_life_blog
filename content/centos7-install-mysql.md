---
title: 一步一步CentOS7 安装 MySQL5.7
categories:
  - 运维
tags:
  - centos7
  - mysql
date: 2020-06-01 01:40:01.965000
---
## 说明：
此文章用于安装Oracle Mysql，而非MariaDB。

## yum源处理
对于yum安装，需要知道yum源，参考官网信息即可：

https://dev.mysql.com/downloads/repo/yum/

同时贴出apt源：

https://dev.mysql.com/downloads/repo/apt/

选择下载，本文贴出两个地址一个是5.7，一个是8.0：

```
# 5.7 
https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
# 8.0 最新版本（2018-12-27）
https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm  
```
根据两个地址的对比，其他版本链接请自行揣测。下载完后进行安装：
```
# 安装rpm包
sudo rpm -Uvh mysql57-community-release-el7-11.noarch.rpm
# 查看yum
yum repolist all | grep mysql
# 显示结果
mysql-connectors-community/x86_64 MySQL Connectors Community
mysql-tools-community/x86_64      MySQL Tools Community
mysql57-community/x86_64          MySQL 5.7 Community Server
安装
sudo yum install mysql-community-server
```

## 启动 MySQL 服务

Mysql5.7版本默认设置了临时密码，需要在启动之后查看运行日志，里面会有临时密码显示：
```
sudo cat /var/log/mysqld.log | grep 'temporary password' 
修改密码

$ mysql -uroot -p  #输入查看到的密码
# 新密码要求符合密码规范。大小写，特殊字符数字等。
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '!QAZ2wsx';
tips:
# mysql初始化data目录
mysqld  --initialize  
# 初始化后，赋予文件权限
chown -R mysql:mysql /var/lib/mysql
```

 后续初始化操作
https://75051685.xyz/archives/mysql-base-config