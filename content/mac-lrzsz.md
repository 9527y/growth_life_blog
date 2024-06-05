---
title: mac终端Iterm2支持rz和sz的解决方案
categories:
  - 运维
tags:
  - lrzsz
date: 2020-12-15 19:31:37.866000
---
在mac下，实现与服务器进行便捷的文件上传和下载操作：

安装支持rz和sz命令的lrzsz：
```
brew install lrzsz
```
在本地`/usr/local/bin/`目录下保存`iterm2-send-zmodem.sh` 和`iterm2-recv-zmodem.sh`两个脚本

两个文件链接：https://github.com/aikuyun/iterm2-zmodem

设置一下两个脚本的权限，一般 chmod 777 就行了
设置Iterm2的Tirgger特性，profiles->default->editProfiles->Advanced中的Tirgger

```
    添加两条trigger，分别设置Regular expression，Action，Parameters，Instant如下：
    1.第一条
        Regular expression: rz waiting to receive.\*\*B0100
        Action: Run Silent Coprocess
        Parameters: /usr/local/bin/iterm2-send-zmodem.sh
        Instant: checked
    2.第二条
        Regular expression: \*\*B00000000000000
        Action: Run Silent Coprocess
        Parameters: /usr/local/bin/iterm2-recv-zmodem.sh
        Instant: checked
```