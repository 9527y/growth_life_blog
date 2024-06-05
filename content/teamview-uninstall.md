---
title: 无法开机 ,报 不可恢复的错误：securityagent无法创建所要求的机制teamviewerauthplugin:start
categories:
  - 杂记
tags:
  - debug
date: 2020-06-30 22:39:31.380000
---
Mac os 10.15  使用CleanMyMac卸载TeamViewer后不能进入系统，报securityagent无法创建所要求的机制teamviewerauthplugin:start；

修复步骤如下

1、找个U盘或移动硬盘

2、下载链接:
	
	链接: https://pan.baidu.com/s/10k-KvYceEy3IN0WKnoxN8w 提取码: ie6h

3、将下载好的TeamViewerAuthPlugin.bundle.tar.gz 复制到U盘或移动硬盘里；

4、按住command + R 开机；

5、先使用硬盘工具，将硬盘挂载上去，加密的硬盘，挂载时会提示你输入密码，输入你开机密码；

6、退出硬盘工具，使用命令终端，进入命令操作界面（点击顶部菜单栏【实用工具】 中的 【终端】）；

7、将刚才U盘或移动硬盘上的`TeamViewerAuthPlugin.bundle.tar.gz`，复制到`/Volumes/Macintosh\ HD/Library/Security/SecurityAgentPlugins/`目录下，`Macintosh\ HD`指的是你硬盘名；

8、解压`TeamViewerAuthPlugin.bundle.tar.gz`，命令：

```
tar -zxvf TeamViewerAuthPlugin.bundle.tar.gz
```

会生成一个`TeamViewerAuthPlugin.bundle`目录；确认 
`/Volumes/Macintosh\ HD/Library/Security/SecurityAgentPlugins/TeamViewerAuthPlugin.bundle`是否已经有了；

9、重启搞定