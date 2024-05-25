---
title: MySQL初始化基础配置
categories:
  - 运维
tags:
  - mysql
date: 2020-06-01 05:44:28.805000
---
## CentOS7安装Mysql5.7
https://75051685.xyz/archives/centos7-install-mysql

修改编码：/etc/my.cnf
```
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci # 不区分大小写
collation-server =  utf8_bin    # 区分大小写
collation-server = utf8_unicode_ci # 比 utf8_general_ci 更准确
lower_case_table_names=2        # 表名区分大小写
max_connections=1000            # 最大连接数
```

## 创建数据库和用户
```
# 创建数据库，编码utf8
CREATE DATABASE <datebasename> CHARACTER SET utf8;

# 创建用户，指定用户名，密码，访问主机
CREATE USER 'username'@'host' IDENTIFIED BY 'password';

# 设置权限
GRANT privileges ON databasename.tablename TO 'username'@'host';

# 显示权限
SHOW GRANTS FOR 'username'@'host';

# 回收用户权限
REVOKE privilege ON databasename.tablename FROM 'username'@'host';

# 删除用户
DROP USER 'username'@'host';
```

用户权限常用的有：

SELECT，INSERT，UPDATE，全部则用 ALL

## 常用sql语句：
```
# 创建数据库，编码utf8
CREATE DATABASE <datebasename> CHARACTER SET utf8;

# 创建用户，指定用户名，密码，访问主机
CREATE USER 'username'@'host' IDENTIFIED BY 'password';

# root用户设置远程访问权限，并且刷新授权
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;  
FLUSH   PRIVILEGES; 

# alexdev用户赋予只读权限
GRANT select  ON *.* TO 'alexdev'@'%' IDENTIFIED BY 'alexdev' WITH GRANT OPTION;
FLUSH   PRIVILEGES; 
如果如下提示，说明需要先重置一下密码。有可能本次登录使用的是临时密码登录。
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```

## 重置密码：
```
mysql> set password = password('123456');
```
## 调整时区

```
cp  /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime

yum install ntp -y && ntpdate pool.ntp.org 
```
## 更改mysql数据文件权限
```
chown -R mysql:mysql /var/lib/mysql

```