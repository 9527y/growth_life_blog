---
title: zuul 网关上传文件大小配置
categories:
  - 开发
tags:
  - springboot
date: 2020-05-26 18:20:18.505000
---
# 关于版本先看pom配置
```
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.2.RELEASE</version>
        <relativePath/>
    </parent>
    <properties>
        <java.version>1.8</java.version>
        <spring-cloud.version>Greenwich.RELEASE</spring-cloud.version>
    </properties>
    -------
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
    </dependency>
```
# 文件大小设置
```
spring:
  application:
    name: dragon-zuul
  servlet:	# 此版本的节点为servlet，不是http 注意一下
    multipart:
      enabled: true # 启用上传处理，默认是true
      file-size-threshold: 1MB   # 当上传文件达到1MB的时候进行磁盘写入
      max-request-size: 10MB	# 设置最大的请求文件的大小
      max-file-size: 10MB	# 设置单个文件的最大长度
```
