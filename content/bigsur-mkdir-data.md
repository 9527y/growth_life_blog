---
title: macOS BigSur下无法在根目录创建/data解决方法
categories:
  - 杂记
tags:
  - mac
date: 2021-01-14 10:51:54.822000
---
```
sudo vim /etc/synthetic.conf
```
添加
```
data	/System/Volumes/Data/data
```

`/System/Volumes/Data/data`目录为目标目录，(data和/xxx/data之间是tab不是空格)。

**注意**：如果`/etc/synthetic.conf`文件只读，不能编辑，请切换到root用户。

重启解决