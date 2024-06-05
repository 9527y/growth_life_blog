---
title: 使用general_log来对Mysql SQL语句监控查看的记录
categories:
  - 运维
tags:
  - mysql
  - usage
date: 2020-05-31 18:27:22.905000
---
查看日志位置：

	show global variables like '%general%';
	+——————+———-+
	|Variable_name|Value|
	+——————+———-+
	|general_log|OFF|
	|general_log_file|/data0/logs/mysql/general.log|
	+——————+———-+

设置 `SET GLOBAL general_log = 'ON';`

使用`tail -f  日志位置` 查看即时日志。


临时开启日志记录

	set global general_log='ON';

这时执行的所有sql都会被记录下来，但是如果重启mysql就会停止记录需要重新设置

查看日志

	tail -f /data0/logs/mysql/general.log

查看全部

	cat /data0/logs/mysql/general.log

查看是否开启binlog

	show variables like “log_bin” ;

查看当前的binlog日志

	show master status ;