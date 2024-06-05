---
title: MySQL查看数据库表容量大小
categories:
  - 运维
tags:
  - mysql
date: 2021-12-3 19:03:25
--- 

## 1.查看所有数据库容量大小

```sql
select table_schema                                 as '数据库',
       sum(table_rows)                              as '记录数',
       sum(truncate(data_length / 1024 / 1024, 2))  as '数据容量(MB)',
       sum(truncate(index_length / 1024 / 1024, 2)) as '索引容量(MB)'
from information_schema.tables
group by table_schema
order by sum(data_length) desc, sum(index_length) desc;
```

## 2.查看所有数据库各表容量大小

```sql
select table_schema                            as '数据库',
       table_name                              as '表名',
       table_rows                              as '记录数',
       truncate(data_length / 1024 / 1024, 2)  as '数据容量(MB)',
       truncate(index_length / 1024 / 1024, 2) as '索引容量(MB)'
from information_schema.tables
order by data_length desc, index_length desc; 
```

## 3.查看指定数据库容量大小

```sql
select table_schema                                 as '数据库',
       sum(table_rows)                              as '记录数',
       sum(truncate(data_length / 1024 / 1024, 2))  as '数据容量(MB)',
       sum(truncate(index_length / 1024 / 1024, 2)) as '索引容量(MB)'
from information_schema.tables
where table_schema = 'mysql';
```

```
数据库	记录数	数据容量(MB)	索引容量(MB)
mysql	8354	2.57	0.24
```

## 4.查看指定数据库各表容量大小

```sql
select table_schema                            as '数据库',
       table_name                              as '表名',
       table_rows                              as '记录数',
       truncate(data_length / 1024 / 1024, 2)  as '数据容量(MB)',
       truncate(index_length / 1024 / 1024, 2) as '索引容量(MB)'
from information_schema.tables
where table_schema = 'mysql'
order by data_length desc, index_length desc;
```

```
数据库	表名	记录数	数据容量(MB)	索引容量(MB)
mysql	help_topic	1292	1.51	0.09
mysql	innodb_index_stats	3173	0.42	0.00
mysql	help_keyword	1016	0.12	0.09
mysql	global_grants	190	0.09	0.00
mysql	help_relation	2167	0.09	0.00
mysql	innodb_table_stats	427	0.07	0.00
mysql	db	2	0.01	0.01
```