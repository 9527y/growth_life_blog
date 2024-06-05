---
title: Mysql5.7主从复制
categories:
  - 运维
tags:
  - mysql
date: 2020-06-02 11:09:54.419000
---
# 环境
Linux:CentOS7
Mysql:5.7
服务器:腾讯云
# 安装
见：https://blog.csdn.net/haxyek/article/details/85273553
安装后可能需要的初始化命令：

    mysqld --initialize-insecure --user=mysql --explicit_defaults_for_timestamp

# 主从复制原理
![主从复制原理](https://img-blog.csdnimg.cn/20190308105033670.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hheHllaw==,size_16,color_FFFFFF,t_70)
从服务器读取主服务器的binlog，进行数据复制。
# 主从复制实践
## 主服务器配置：
### 第一步：修改my.cnf文件

[mysqld]段增加
```
#启动而进制日志
log-bin=mysql-bin
#服务器唯一ID，一般取IP最后一段
server-id=83
```
### 第二步：重启mysql服务
```
systemctl restart mysqld
```
### 第三步：主机给从机授权备份权限
登录到主机的mysql
```
myysql> GRANT REPLication slave on *.* to '从机用户名'@'从机ip地址' identified by '从机密码';
```
示例
```
myysql> GRANT REPLication slave on *.* to 'slave'@'172.32.1.16' identified by 'slave_password';
```
### 第四步：刷新权限
```
mysql>flush privileges;
```
### 第五步：查询master的状态
```
mysql>show master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000002 |      601 |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
```
## 从服务器配置
### 第一步：修改my.cnf文件
```
[mysqld]
server-id=16
```
### 第二步：重启并登录到mysql进行配置从服务器配置
```
mysql> change master to 
 master_host = '172.32.1.83',
 master_port=3306,
 master_user='slave',
 master_password='slave_password',
 master_log_file='mysql-bin.000002',
 master_log_pos=601
```

> 注意：
> master_port:mysql服务端口号，
> master_user:为执行同步数据库账户
> master_log_file:为主机show master status看到的File的值
> master_log_pos:为主机show master status看到的position的值，不带引号
> 如果是想延迟同步可以使用master_delay = 1800 ，延迟1800秒

### 第三步：检查从服务器的复制功能
```
mysql> start slave;
```
### 第四步：检查从服务器复制功能状态：
```
mysql>show slave status \G;
………………………(省略若干)………………………
             Slave_IO_Running: Yes  # 此处应为YES
            Slave_SQL_Running: Yes # 此处应为YES
………………………(省略若干)………………………
```

## 测试
以上步骤完成后，往主机中插入数据，看看从机中是否有数据。