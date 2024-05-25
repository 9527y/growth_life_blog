---
title: nacos 配置中心的使用
categories:
  - 开发
tags:
  - nacos
date: 2020-07-16 19:44:53.218000
---

## pom配置

在根pom.xml中设置

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.1.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

在项目中增加依赖：

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

## 设置配置

Nacos Config 使用 DataId 和 GROUP 确定一个配置。
![](https://camo.githubusercontent.com/b376cc3d409a74cd89b80cecbf8c1bde5ef58580/68747470733a2f2f696d672e616c6963646e2e636f6d2f7466732f544231596c693362555931674b306a535a464d58586157635658612d323433362d313133382e706e67)


```properties
Data ID:    nacos-config.properties

Group  :    DEFAULT_GROUP

配置格式:    Properties

配置内容：   user.name=nacos-config-properties
            user.age=90
```

- DataId 默认取 application-name，Group默认：default_group，类型后缀默认：*.properties

## 使用

增加`bootstrap.propertie`，配置文件来配置 Nacos Server 地址


```properties
spring.application.name=base-provider
spring.cloud.nacos.config.server-addr=peer1:8848,peer2:8848,peer3:8848
logging.level.com.alibaba.nacos.client.naming=error
logging.level.com.alibaba.nacos.config.log.level=error 
logging.level.com.alibaba.nacos.naming.log.level=error 
# 可以使用 spring.cloud.nacos.config.file-extension=yaml 
```

这里我们使用 @Value 注解来将对应的配置注入到 HeartController 的 userName 和 age 字段，并添加 @RefreshScope 打开动态刷新功能


- 使用方法一：

```java
@RequestMapping("/heart")
@RefreshScope
@RestController
public class HeartController {

    @Value("${server.port}")
    private int port;


    @Value("${user.name}")
    String userName;

    @Value("${user.age}")
    int age;
    
......

```

使用方法二：

```java
@Component
@RefreshScope
@Data
public class NacosTestConfig {

    @Value("${user.name}")
    private String userName;

    @Value("${user.age}")
    private int age;
}

```


## 启动脚本配置文件
```bash
#!/bin/bash
nohup  java -jar *.jar --spring.profiles.active=test &
```




## 更多详细文档
- [Spring Cloud Alibaba Nacos Config](https://github.com/alibaba/spring-cloud-alibaba/blob/master/spring-cloud-alibaba-docs/src/main/asciidoc-zh/nacos-config.adoc)
- [Nacos Config Example](https://github.com/alibaba/spring-cloud-alibaba/blob/master/spring-cloud-alibaba-examples/nacos-example/nacos-config-example/readme-zh.md)