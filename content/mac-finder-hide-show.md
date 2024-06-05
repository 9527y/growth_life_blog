---
title: Mac操作系统下怎么显示隐藏文件
categories:
  - 杂记
tags:
  - mac
date: 2020-12-17 21:07:47.580000
---
第一步：打开「终端」应用程序。

第二步：输入如下命令：

```
defaults write com.apple.finder AppleShowAllFiles -boolean true ;
killall Finder
```

第三步：按下「Return」键确认。

现在你将会在 Finder 窗口中看到那些隐藏的文件和文件夹了。

如果你想再次隐藏原本的隐藏文件和文件夹的话，将上述命令替换成

```
defaults write com.apple.finder AppleShowAllFiles -boolean false ;
killall Finder
```