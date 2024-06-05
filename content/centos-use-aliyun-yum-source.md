---
title: CentOS7使用阿里yum源
categories:
  - 运维
tags:
  - centos7
  - yum
date: 2020-10-12 16:31:14.843000
---
目录
[toc]

## 禁用 yum插件 fastestmirror

1. 修改插件的配置文件

```
cp /etc/yum/pluginconf.d/fastestmirror.conf /etc/yum/pluginconf.d/fastestmirror.conf.bak 
vi  /etc/yum/pluginconf.d/fastestmirror.conf  
```
`enabled = 1`由1改为0，禁用该插件

2. 修改yum的配置文件

```
cp /etc/yum.conf /etc/yum.conf.bak
vi /etc/yum.conf
```
`plugins=1`改为0，不使用插件

## 获取阿里云 repo
```
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

cp /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel.repo.bak
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

## 清理缓存，重建缓存 
```
yum clean all
yum makecache
```
完工。