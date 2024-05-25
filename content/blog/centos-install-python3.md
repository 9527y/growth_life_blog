---
title: Centos7 安装 Python3.8
categories:
  - 运维
tags:
  - centos7
  - python
date: 2020-10-12 16:33:26.186000
---
目录[TOC](index)

## Step 1 – 安装前准备
可以切换到阿里云yum源：
```
sudo yum -y update
```
必备包
```
sudo yum -y install wget yum-utils gcc openssl-devel bzip2-devel libffi-devel zlib zlib-devel
```

## Step 2 – 源码下载编译
```
cd /tmp/
wget https://www.python.org/ftp/python/3.8.6/Python-3.8.6.tgz
tar xzf Python-3.8.6.tgz
cd Python-3.8.6
```

```
sudo ./configure --prefix=/opt/python38 --enable-optimizations --with-lto --with-system-ffi --with-computed-gotos --enable-loadable-sqlite-extensions
sudo make -j "$(nproc)"
sudo make altinstall
```
如果提示：
```
  /usr/local/bin/python: can't decompress data; zlib not available
```

安装目录/Modules/Setup ，将`#zlib zlibmodule.c -I$(prefix)/include -L$(exec_prefix)/lib -lz`的注释去掉。

## Step 3 - 安装后操作
```
sudo ln -s /opt/python38/bin/python3.8 /opt/python38/bin/python3
sudo ln -s /opt/python38/bin/python3.8 /usr/bin/python38
sudo ln -s /opt/python38/bin/python3.8 /opt/python38/bin/python
sudo ln -s /opt/python38/bin/python3.8-config /opt/python38/bin/python-config
sudo ln -s /opt/python38/bin/pydoc3.8 /opt/python38/bin/pydoc
sudo ln -s /opt/python38/bin/idle3.8 /opt/python38/bin/idle

sudo ln -s /opt/python38/bin/pip3.8 /opt/python38/bin/pip3
sudo ln -s /opt/python38/bin/pip3.8 /opt/python38/bin/pip
```

## Step 4 -  创建虚拟环境(venv)
```
sudo /opt/python38/bin/python -m venv /opt/pyenv

source /opt/pyenv/bin/activate
```
