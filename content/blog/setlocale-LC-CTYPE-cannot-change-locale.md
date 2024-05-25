---
title: 在Mac下远程登录Linux时,提示cannot change locale (UTF-8) No such file or directory
categories:
  - 运维
tags:
  - ssh
date: 2021-05-05 20:30:12
---
## 问题描述
- Mac下设置第一语言为English
- 在Terminal或者iTerm2上登录远端Linux时，Linux的prompt提示 `setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory`
- 登录Linux后无法正常显示中文
## 原因
1. Mac下设置为英文后，locale字符集默认是”C”，Terminal或者iTerm2中有选项会自动设置LC_CTYPE或者LC_LANG为UTF-8
2. Mac下ssh客户端的配置文件/etc/ssh/ssh_config中，会尝试设置本地的LANG到远端服务器中。
3. 远端Linux服务器，没有UTF-8的字符集，就导致了setlocale的警报
## 解决办法
为了登录而来，修改每个服务器的字符集，操作上是不可行的。最简单的办法就是修改Mac本地的ssh客户端配置，不要将LANG设置发送到服务器端。
打开ssh配置文件，`sudo vim /etc/ssh/ssh_config`, 注释掉如下几行

```
Host *
	SendEnv LANG LC_*
```

重新ssh到服务器，就不会再有setlocale的告警了。