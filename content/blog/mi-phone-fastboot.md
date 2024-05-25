---
title: 如何用Mac线刷小米手机
categories:
  - 运维
tags:
  - usage
date: 2020-05-22 18:26:04.503000
---
目录
[TOC](目录)
## ROM下载地址
对于老款小米，需要使用低版本的ROM才不会卡，这里提供两个ROM历史下载地址，都为非官方的，因为官方的不全。

- [https://miuiver.com/](https://miuiver.com/)
- [https://roms.miuier.com/](https://roms.miuier.com/)

##  mac刷机工具 adb
安装
```
brew cask install android-platform-tools
```

安装好后配置一下环境变量。
在`/etc/profile`中加入。
```
export PATH=~/android-sdks/platform-tools:~/android-sdks/tools:$PATH
```

运行`fastboot --version`检测是否成功安装并配置好环境。 


## 开始刷机
1. 打开手机调试模式，并手机连接到电脑
2. 进入fastboot模式：

- 手机已开机使用：`adb reboot bootloader`
- 没有开机：开机键+音量下长按
3. 查看当前手机设备名：
```shell
$ fastboot devices
4e46d6ca	fastboot
```
`4e46d6ca`为当前设备名
4. 控制台进入到解压后的目录：
运行脚本：
```
~/Downloads/kenzo_images_V7.5.2.0.LHOCNDE_20160629.0000.25_5.1_cn ⌚ 14:13:59
$ sh flash_all.sh -s 4e46d6ca
```

## tips
实验机器：红米Note3全网通，刷的ROM kenzo_images_V7.5.2.0