---
title: 镜像版本说明
categories:
  - 运维
tags:
  - null
date: 2022-03-16 00:43:22
---
不同的tag表示给予不同的base image。

debian linux 版本代号

```
Debian 11（"bullseye"） — 下一代发布时间尚未确定
Debian 10（"buster"）- 当前的稳定版（stable）
Debian 9（"stretch"）- 旧的稳定版（oldoldstable）
Debian 8（"jessie"） — 更旧的稳定版（oldoldstable）
Debian 7（"wheezy"） — 被淘汰的稳定版
Debian 6.0（"squeeze"） — 被淘汰的稳定版
Debian GNU/Linux 5.0（"lenny"） — 被淘汰的稳定版
Debian GNU/Linux 4.0（"etch"） — 被淘汰的稳定版
Debian GNU/Linux 3.1（"sarge"） — 被淘汰的稳定版
Debian GNU/Linux 3.0（"woody"） — 被淘汰的稳定版
Debian GNU/Linux 2.2（"potato"） — 被淘汰的稳定版
Debian GNU/Linux 2.1（"slink"） — 被淘汰的稳定版
Debian GNU/Linux 2.0（"hamm"） — 被淘汰的稳定版
```
- alpine：本意是高山的，是一个面向安全的轻型 Linux 发行版。和Debian一样都是Linux的一种发行版本。此版本系统特点是小巧、安全、简单、适合容器使用。
- buster：Debian版本，类似的还有Jessie（2015年发行的）、wheezy （2013年发行）
- slim：镜像的瘦身版。大概意思是通过动态和静态的分析，去除不必要的依赖，缩减镜像的大小

​前面说过，不同的tag是基于不同的base image构建的镜像。上面的alpine是基于该种Linux构建的，buster、stretch、jessie是基于相应的debian Linux版本构建的。slim是特殊的瘦身版。

此外某些镜像本身集成了jdk，jdk有多种所以根据jdk的版本不同，打上了不同的tag。以下说明jdk的版本号

- jdk：通常指的是oracle维护的java develop kit。
- openjdk：是jdk的开放的原始版本。
- adoptopenjdk：oracle从sun收购java后，出于商业目的，2019年后不在免费维护。托管给了社区。adopt本身也有托管、收养的意思。该版本由社区维护，大概跟openJDK类似。

​ 总之一句话，Java历史上的爱恨情仇导致了现在复杂的局面。

openjdk是jdk的开放原始码版本，以GPL协议的形式放出。在JDK7的时候，openjdk已经成为jdk7的主干开发，sun jdk7是在openjdk7的基础上发布的，其大部分原始码都相同，只有少部分原始码被替换掉。使用JRL(JavaResearch License，Java研究授权协议)发布。

## 关于JDK和OpenJDK的区别，可以归纳为以下几点：

- 授权协议的不同

openjdk采用GPL V2协议放出，而JDK则采用JRL放出。两者协议虽然都是开放源代码的，但是在使用上的不同在于GPL V2允许在商业上使用，而JRL只允许个人研究使用。

- OpenJDK不包含Deployment（部署）功能

部署的功能包括：Browser Plugin、Java Web Start、以及Java控制面板，这些功能在Openjdk中是找不到的。

- OpenJDK源代码不完整

这个很容易想到，在采用GPL协议的Openjdk中，sun jdk的一部分源代码因为产权的问题无法开放openjdk使用，其中最主要的部份就是JMX中的可选元件SNMP部份的代码。因此这些不能开放的源代码将它作成plug，以供OpenJDK编译时使用，你也可以选择不要使用plug。而Icedtea则为这些不完整的部分开发了相同功能的源代码(OpenJDK6)，促使OpenJDK更加完整。

- 部分源代码用开源代码替换

由于产权的问题，很多产权不是SUN的源代码被替换成一些功能相同的开源代码，比如说字体栅格化引擎，使用Free Type代替。

- openjdk只包含最精简的JDK

OpenJDK不包含其他的软件包，比如Rhino Java DB JAXP……，并且可以分离的软件包也都是尽量的分离，但是这大多数都是自由软件，你可以自己下载加入。

- 不能使用Java商标

这个很容易理解，在安装openjdk的机器上，输入“java -version”显示的是openjdk，但是如果是使用Icedtea补丁的openjdk，显示的是java。（未验证）