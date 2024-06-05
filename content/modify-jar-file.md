---
title: jar：解压jar包更改文件后再重新打包
categories:
  - 技术
tags:
  - java
date: 2022-01-06 22:52:17
---

1、阻止jar打包时重新生成清单列表， -M 不生成配置清单，这样还可以使用maven生成的配置清单也就是MANIFEST.MF

```shell
jar -cfM xxx.jar *
```

2、jar打包时不进行压缩 -0

```shell
jar -cfM0 xxx *
```

3、不用解压后替换文件再压缩，如下命令更新更简单：

```shell
jar uf xxx.jar BOOT-INF/classes/application-dev.yml
```

4、解压命令：

```shell
jar -xf xxx.jar
```

## jar命令参数：

jar命令格式：jar {c t x u f }[ v m e 0 M i ][-C 目录]文件名...
其中{ctxu}这四个参数必须选选其一。[v f m e 0 M i ]是可选参数，文件名也是必须的。

```
　-c　创建新的 JAR 文件包
　-t　列出 JAR 文件包的内容列表
　-x　展开 JAR 文件包的指定文件或者所有文件
　-u　更新已存在的 JAR 文件包 (添加文件到 JAR 文件包中)
　[vfm0M] 中的选项可以任选，也可以不选，它们是 jar 命令的选项参数
　-v　生成详细报告并打印到标准输出
　-f　指定 JAR 文件名，通常这个参数是必须的
　-m　指定需要包含的 MANIFEST 清单文件
　-0　只存储，不压缩，这样产生的 JAR 文件包会比不用该参数产生的体积大，但速度更快
　-M　不产生所有项的清单（MANIFEST〕文件，此参数会忽略 -m 参数
　[jar-文件] 即需要生成、查看、更新或者解开的 JAR 文件包，它是 -f 参数的附属参数
　[manifest-文件] 即 MANIFEST 清单文件，它是 -m 参数的附属参数
　[-C 目录] 表示转到指定目录下去执行这个 jar 命令的操作。它相当于先使用 cd 命令转该目录下再执行不带 -C 参数的 jar 命令，它只能在创建和更新 JAR 文件包的时候可用。
```
