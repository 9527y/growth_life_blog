---
title: 'MyBatis获取插入记录的自增长字段值'
tags:
  - java
categories:
  - 开发
date: 2020-06-02 19:14:57.782000
---
# MyBatis Plus
MyBatis plus 的insert语句，默认会给参数回加上自增主键的值。
# MyBatis
再写xml时候注意，需要指定参数类型，parameterType，并加上`useGeneratedKeys="true" ` 和指定主键字段：`keyProperty`
```
<insert id="insert" 
	parameterType="com.helloworld.User"  <!--指定参数类型-->
		useGeneratedKeys="true"
		keyProperty="id"   <!--指定主键字段-->
		>
```
