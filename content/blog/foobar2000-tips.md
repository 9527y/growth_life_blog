---
title: foobar2000播放的一些使用技巧
categories:
  - 杂记
tags:
  - usage
date: 2020-05-26 18:31:03.137000
---
听ape音乐，cue乱码或者找不到文件，ftp无法上传的解决办法：
## 第一种情况，找不到文件
我用iPhone手机播放的时候，提示找不到，看了一下提示，是路径问题。将ape文件，和cue放在同一目录；同时将`FILE`使用相对路径，`./`开头加文件名即可。
```
PERFORMER "BEYOND"
TITLE "BEYOND珍藏版CD2"
FILE "./BEYOND.CD2.ape" WAVE
  TRACK 01 AUDIO
```
## 第二种情况，找到文件之后，播放列表是乱码
如果用记事本文件打开是正常的显示，只是放入手机中是乱码，则可以更改文件编码格式即可。推荐使用vscode，简单易用。更改编码为`UTF-8 with BOM`

![切换文件编码](https://img-blog.csdnimg.cn/20200430103026238.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hheHllaw==,size_16,color_FFFFFF,t_70)
## 无法上传到媒体库
 对于IPhone使用foobar2000，无法上传情况实际上是foobar安装后，未有网络权限，简单的办法可以触发Foobar2000在iPhone上网络权限开关的方法如下：在Browse界面，选择Media Servers，然后Add new添加一个新的服务器，例如随便输入一个ftp服务器，比如： `ftp://192.168.2.1`
 ![触发网络权限开关](https://img-blog.csdnimg.cn/2020043010362948.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hheHllaw==,size_16,color_FFFFFF,t_70)
 
接下来就会弹出网络授权按钮，允许后，再去开启FTP服务，就可以通过电脑端访问Foobar2000手机版的FTP服务并且传文件到手机了。
