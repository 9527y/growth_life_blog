---
title: 各个系统查看端口情况的方法
categories:
  - 开发
tags:
  - tools
date: 2020-06-02 11:56:08.709000
---
# 查看本机占用情况
## windows
windows常用命令netstat，与find组合使用
```cmd
netsat -an | find "8080"
```
## mac
```shell
lsof -i 8090
```
## linux
```shell
netstat -anptl | grep 8080
```
# 查看远程端口是否开启
## nc
```shell
nc -v -n 127.0.0.1 22
```
 ## wget
```shell
 wget 192.168.1.1:6379
```
## curl
```shell
curl 192.168.1.1:6379
```