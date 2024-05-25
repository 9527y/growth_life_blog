---
title: mysql bin-log 误操作数据修复
categories:
  - 运维
tags:
  - mysql
date: 2020-06-01 18:25:23.855000
---

# 先说原理
基于全量备份，加bin-log，将数据恢复到误操作之前节点数据，然后跳过误操作，执行后面操作。使用binlog文件转成sql执行，导入到数据库。
## 查看binlog文件
如果有多个binlog日志也可以在Mysql命令行下查看当前binlog、切割binlog日志。切割完成binlog再次查看就会看到新的日志写入到新的binlog文件中。

```
mysql> show master status;
+------------------+-----------+--------------+------------------+-------------------+
| File             | Position  | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+-----------+--------------+------------------+-------------------+
| mysql-bin.000001 | 343629748 |              |                  |                   |
+------------------+-----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)

mysql> flush logs;
Query OK, 0 rows affected (0.01 sec)
```

## 找到binlog中错误的语句

使用mysqlbinlog命令加参数，可以指定时间段，和pos节点：

```
--start-position    起始点
--stop-position     结束点（不包含）
--start-datetime=“2017--11-01 00:00:00”     起始时间
--stop-datetime="2017-11-11 00:00:00"       终止时间
-d db_name          指定数据库
-v                  是显示出一些sql的信息 
-vv                 则是多一些注释性的东西
--base64-output=DECODE-ROWS 这个是把sql解码出来
```

可以binlog日志中找到错误语句执行的时间点，分别恢复错误语句前后的binlog日志为sql。也可以跳过此步，直接恢复整个binlog日志为sql，然后打开sql文件，删除错误语句。

- 可以根据时间点来确定误操作的确切时间

```shell
mysqlbinlog  --no-defaults --database=order  --start-datetime='2019-11-02 3:08' --stop-datetime='2019-11-02 23:08' --base64-output=DECODE-ROWS mysql-bin.000004 > neworder.sql
```

- 根据position确定误操作之前的节点

```shell
mysqlbinlog  --no-defaults --database=order --start-position=756577992  --stop-position=775926228 --base64-output=DECODE-ROWS mysql-bin.000004 > neworder.sql
```

- 过滤误删除的sql

```shell
# sudo mysqlbinlog --base64-output=DECODE-ROWS -v -d ids mysql-bin.000001 | grep --ignore-case -A3 -B4 '错误的sql语句'
```

## 恢复数据时
恢复数据准备前确认已经存在如下文件：

1. 误操作前的完整备份
2. 全量备份到误操作前的执行sql
3. 误操作后的执行sql

进行数据恢复时开启读锁 

```sql
> flush tables with read lock;
```
然后依次还原1，2，3；最后把数据库锁解开

```sql
> unlock tables;
```


