---
title: '解决MacOS升级后出现xcrun: error: invalid active developer path, missing xcrun的问题'
categories:
  - 开发
tags:
  - mac
date: 2020-12-28 00:18:31.668000
---
升级Mac系统后，使用shell命令，或者node之类的报错：
```
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun

```
解决方法，重装xcode command line：
```shell
xcode-select --install
```

如果没有解决问题，执行以下命令

```
sudo xcode-select -switch /
```

