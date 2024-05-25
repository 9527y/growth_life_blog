---
title: CentOS7修改服务器ssh端口号
categories:
  - 运维
tags:
  - centos7
date: 2020-05-31 18:11:45.721000
---
# 版本
CentOS7
# 一般情况
编辑sshd的配置文件，增加端口既可
```
vi /etc/ssh/sshd_config
----
Port 22
Port 1022
```
然后重启服务
```
systemctl status sshd.service
```
# 开启了SELinux策略
如果开启了SELinux策略，则可以使用下面命令查看端口状态：
```
semanage port -l | grep ssh
```
增加访问端口
```
semanage port -a -t ssh_port_t -p tcp 1022
```
# 防火墙配置
```
firewall-cmd --list-all
firewall-cmd --zone=public --add-port=1022/tcp --permanent
firewall-cmd --reload
```