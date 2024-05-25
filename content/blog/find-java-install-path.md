---
title: 查看Java安装路径
categories:
  - 开发
tags:
  - java
  - path
date: 2020-10-22 11:12:55.419000
---
### CentOS
```
[root@ai-python ~]# which java
/usr/bin/java

[root@ai-python ~]# ll -a /usr/bin/java
lrwxrwxrwx. 1 root root 22 10月 20 09:35 /usr/bin/java -> /etc/alternatives/java

[root@ai-python ~]# ll -a /etc/alternatives/java
lrwxrwxrwx. 1 root root 41 10月 20 09:35 /etc/alternatives/java -> /usr/java/jdk1.8.0_191-amd64/jre/bin/java
```
### MacOS

```
 $ /usr/libexec/java_home -V
Matching Java Virtual Machines (3):
    1.8.271.09 (x86_64) "Oracle Corporation" - "Java" /Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
    1.8.0_271 (x86_64) "Oracle Corporation" - "Java SE 8" /Library/Java/JavaVirtualMachines/jdk1.8.0_271.jdk/Contents/Home
    1.8.0_261 (x86_64) "Oracle Corporation" - "Java SE 8" /Library/Java/JavaVirtualMachines/jdk1.8.0_261.jdk/Contents/Home
/Library/Java/JavaVirtualMachines/jdk1.8.0_271.jdk/Contents/Home
```