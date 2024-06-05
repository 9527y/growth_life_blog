---
title: php-fpm 报错500排查
categories:
  - 运维
tags:
  - usage
  - php

date: 2020-05-31 18:33:22.639000
---
[TOC](目录)

# php-fpm 报错500
php-fpm报错500，使用nginx转发，同时有没有日志。

思路将错误暴露出来：

## 使用phpinfo将php.ini的路径找出
新建一个页面，内容如下：
```php
<?php phpinfo();
?>
```
找到php.ini的位置：

```
/usr/local/php/etc/php.ini
```
## 编辑php.ini
查找 **display_errors**，将配置改为**On**，即为显示错误。

```
display_errors = On
```


**记得处理好问题后改回去。**
