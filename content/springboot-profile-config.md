---
title: Springboot 指定profile配置文件
categories:
  - 开发
tags:
  - springboot
date: 2020-06-01 21:48:38.431000
---

Maven启动指定Profile通过-P，如mvn spring-boot:run -Ptest，但这是Maven的Profile。

如果要指定spring-boot的spring.profiles.active，则必须使用mvn spring-boot:run -Drun.profiles=test

如果使用命令行直接运行jar文件，则使用java -jar -Dspring.profiles.active=test demo-0.0.1-SNAPSHOT.jar

注意：**-Dspring.profiles.active=test**的位置。

如果使用开发工具，运行Application.java文件启动，则增加参数--spring.profiles.active=test