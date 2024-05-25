---
title: java:too many open files 解决办法
categories:
  - 开发
tags:
  - tools
  - java
  - debug
date: 2020-06-13 10:43:22.170000
---
[TOC]
## 产生原因

Linux 中,文件是一个字节序列。这种简单但强大的定义和它的实现使得系统中的所有东西都可以用文件来表示。这里提示的打开文件过多，不仅仅是普通的文件，也包括通讯链接socket，监听端口。这个错误通常是句柄数超出系统限制。

引起的原因就是进程在某个时刻打开了超过系统限制的文件数量以及通讯链接数。

- 通过命令`ulimit -a`可以查看当前系统设置的最大句柄数是多少：

```shell
$ ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 15065
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 15065
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited

```
open files那一行就代表系统目前允许单个进程打开的最大句柄数，这里是1024。 

- 使用命令`lsof -p 进程id`可以查看单个进程所有打开的文件详情，

```shell
$lsof -p 18260
COMMAND   PID USER   FD      TYPE             DEVICE  SIZE/OFF     NODE NAME
java    18260 root  cwd       DIR              253,0        95 51010140 /usr/local/www/auth-provider
java    18260 root  rtd       DIR              253,0       236       64 /
java    18260 root  txt       REG              253,0      8534 50882672 /usr/java/jdk1.8.0_191-amd64/jre/bin/java
java    18260 root  mem       REG              253,0    283312 17363896 /usr/java/jdk1.8.0_191-amd64/jre/lib/amd64/libsunec.so
java    18260 root  mem       REG              253,0     93944 17363886 /usr/java/jdk1.8.0_191-amd64/jre/lib/amd64/libnio.so
java    18260 root  mem       REG              253,0     68192    51221 /usr/lib64/libbz2.so.1.0.6
java    18260 root  mem       REG              253,0    157424    51208 /usr/lib64/liblzma.so.5.2.2
…	…	…	…	…	…	…	…

```
- 使用命令`lsof -p 进程id | wc -l`可以统计进程打开了多少文件

```shell
$ lsof -p 18260| wc -l
317
```
如果文件数过多使用`lsof -p 进程id`命令无法完全查看的话，可以使用`lsof -p 进程id > openfiles.log`将执行结果内容输出到日志文件中查看。 

## 解决方法

- 增大允许打开的文件数——命令方式
```
ulimit -n 2048
```
这样就可以把当前用户的最大允许打开文件数量设置为2048了，但这种设置方法在重启后会还原为默认值。 
`ulimit -n命令` 非root用户只能设置到4096。想要设置到更大需要sudo权限或者root用户。

- 增大允许打开的文件数——修改系统配置文件


修改配置文件`/etc/security/limits.conf`在最后加入

```
* soft nofile 4096  
* hard nofile 4096  
```
或者只加入
```
 * - nofile 8192
```

最前的 * 表示所有用户，可根据需要设置某一用户，例如

```
roy soft nofile 8192  
roy hard nofile 8192  
```
注意”nofile”项有两个可能的限制措施。就是项下的hard和soft。 要使修改过得最大打开文件数生效，必须对这两种限制进行设定。 如果使用”-“字符设定, 则hard和soft设定会同时被设定。

- 检查程序问题

如果你对你的程序有一定的解的话，应该对程序打开文件数(链接数)上限有一定的估算，如果感觉数字异常，请使用第一步的lsof -p 进程id > openfiles.log命令，获得当前占用句柄的全部详情进行分析。

## 我遇到的问题

在分析 lsof 返回的详情中，发现socket数据一直增加，觉得是某个资源未释放，导致的。在http请求方面使用`hutool-v4.5.0版本`，此版本的http请求工具类有缺陷。升级至最新，即可解决。
