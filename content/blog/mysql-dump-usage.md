---
title: 使用mysqldump导出mysql表结构和表数据
categories:
  - 运维
tags:
  - mysql
  - tools
  - mysqldump
date: 2020-05-25 18:19:28.283000
---

# 创建备份用户
```
create user dumper@'127.0.0.1' identified by '12345678';
```

# 赋权限
```
grant select on *.* to dumper@'127.0.0.1';
grant lock tables on *.* to dumper@'127.0.0.1';
```

# 备份脚本
```backup_db.sh
#!/bin/bash
mkdir /tmp/`date +%y%m%d`
mysqldump -h127.0.0.1 -u dumper -p12345678 dbname > /tmp/`date +%y%m%d`/db.sql
```

# crontab定时任务
凌晨三点执行脚本。
```
0 3 * * * /opt/software/backup_db.sh  >/dev/null 2>&1
```

# MySQL 备份包含 emoji 表情的数据

在执行备份命令时，指定字符集即可（@see mysqldump --help）

```
$ mysqldump -uroot -p123456 --default-character-set=utf8mb4 db_name > db_name_bak.sql
```

# 常见用法

命令行下具体用法如下： 
```
mysqldump -u用戶名 -p密码 -d 数据库名 表名 > 脚本名;
```
导出整个数据库结构和数据
```
mysqldump -h localhost -uroot -p123456 database > dump.sql
```

导出单个数据表结构和数据
```
mysqldump -h localhost -uroot -p123456  database table > dump.sql
```

导出整个数据库结构（不包含数据）
```
mysqldump -h localhost -uroot -p123456  -d database > dump.sql
```

导出单个数据表结构（不包含数据）
```
mysqldump -h localhost -uroot -p123456  -d database table > dump.sql
```