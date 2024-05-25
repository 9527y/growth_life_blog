---
title: CentOS7 重置root密码
categories:
  - 运维
tags:
  - centos7
date: 2020-05-24 18:15:34.938000
---
1 - 在启动grub菜单，选择编辑选项启动
![grub](https://img-blog.csdnimg.cn/20190515152550717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hheHllaw==,size_16,color_FFFFFF,t_70)
2 - 按键盘e键，来进入编辑界面
![CentOS7 重置root密码](https://img-blog.csdnimg.cn/20190515152728882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hheHllaw==,size_16,color_FFFFFF,t_70)
3 - 找到Linux 16的那一行，将ro改为`rw init=/sysroot/bin/sh`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515152708255.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hheHllaw==,size_16,color_FFFFFF,t_70)
4 - 现在按下 `Control+x`，使用单用户模式启动
![CentOS7 重置root密码](https://img-blog.csdnimg.cn/20190515152638473.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hheHllaw==,size_16,color_FFFFFF,t_70)

5 - 现在，可以使用下面的命令访问系统 `chroot /sysroot`

6 - 重置密码 `passwd root`

7 - 更新系统信息 `touch /.autorelabel`

8 - 退出chroot `exit`

9 - 重启你的系统 `reboot`