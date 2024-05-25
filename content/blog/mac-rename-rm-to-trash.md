---
title: mac改写rm命令：移到废纸篓
categories:
  - 运维
tags:
  - mac
date: 2021-06-28 11:08:51
---
使用trash脚本替换rm命令，它的实质是调用finder的api进行删除操作，也就是移除到废纸篓，也就拥有了废纸篓的恢复源文件功能。

## 安装trash

使用homebrew安装trash
```shell
$ brew install trash
```
此时已经可以使用trash -fr filename，命令与rm一样。但是由于已经习惯性地使用rm命令，改成trash还是有时会习惯性地使用rm删除，因此将rm替换为trash

## 使用trash替换rm命令
在(~/.bash_profile|~/.zshrc)文件中将rm指向trash，添加下列语句。
```shell
## 安装了一个 trash 命令，替代 rm 命令，被删除的文件会放到垃圾桶
alias rm="trash"
```
使用source命令生效。

此时使用rm命令删除文件后会发现文件在废纸篓里了，而且可以使用放回原处的功能。

