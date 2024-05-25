---
title: CentOS7设置内核启动顺序
categories:
  - 运维
tags:
  - CentOS
date: 2021-04-15 09:12:47
---

1、查看设备上安装了几个内核
```
$ cat /boot/grub2/grub.cfg |grep menuentry

if [ x"${feature_menuentry_id}" = xy ]; then
  menuentry_id_option="--id"
  menuentry_id_option=""
export menuentry_id_option
menuentry 'CentOS Linux (3.10.0-1160.24.1.el7.x86_64) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-3.10.0-1127.el7.x86_64-advanced-1760fedb-5552-4ad8-97c1-d1cf555ce12a' {
menuentry 'CentOS Linux (5.4.111-1.el7.elrepo.x86_64) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-3.10.0-1127.el7.x86_64-advanced-1760fedb-5552-4ad8-97c1-d1cf555ce12a' {
menuentry 'CentOS Linux (3.10.0-1127.el7.x86_64) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-3.10.0-1127.el7.x86_64-advanced-1760fedb-5552-4ad8-97c1-d1cf555ce12a' {
menuentry 'CentOS Linux (0-rescue-c37a4d8b3c8d46958beaeafb2f03d2ba) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-0-rescue-c37a4d8b3c8d46958beaeafb2f03d2ba-advanced-1760fedb-5552-4ad8-97c1-d1cf555ce12a' {

```
2、查看当前内核

```
$ grub2-editenv list
saved_entry=CentOS Linux (5.4.111-1.el7.elrepo.x86_64) 7 (Core)
```

3、修改默认启动的内核

```
$ grub2-set-default 'CentOS Linux (5.4.111-1.el7.elrepo.x86_64) 7 (Core)'
```