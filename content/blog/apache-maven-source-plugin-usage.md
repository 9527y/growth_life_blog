---
title: 'Apache Maven Source Plugin-Maven打包源码到私服'
categories:
  - 开发
tags:
  - maven
  - tools
date: 2020-06-02 14:57:55.879000
---

# 使用插件Apache Maven Source Plugin
官网说明：https://maven.apache.org/plugins/maven-source-plugin/index.html
# Goals Overview
 source:jar 是默认的。
The Source Plugin has five goals:

* source:aggregate aggregrates sources for all modules in an aggregator project.
* source:jar is used to bundle the main sources of the project into a jar archive.
* source:test-jar on the other hand, is used to bundle the test sources of the project into a jar archive.
* source:jar-no-fork is similar to jar but does not fork the build lifecycle.
* source:test-jar-no-fork is similar to test-jar but does not fork the build lifecycle.
# POM Demo
```xml
<project>
  ...
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-source-plugin</artifactId>
        <version>3.0.1</version><!--版本号查询官网最新版-->
        <configuration>
          <outputDirectory>/absolute/path/to/the/output/directory</outputDirectory>
          <finalName>filename-of-generated-jar-file</finalName>
          <attach>false</attach>
        </configuration>
      </plugin>
    </plugins>
  </build>
  ...
</project>
```

# 快速使用
1. 配置好发布的私服地址。

```xml
<project>
  ...
    <distributionManagement>
        <repository>
            <id>deploymentRepo</id>
            <name>Nexus Release Repository</name>
            <url>http://192.168.1.53:8081/repository/maven-releases/</url>
        </repository>
        <snapshotRepository>
            <id>snapshot</id>
            <name>snapshot</name>
            <url>http://192.168.1.53:8081/repository/maven-snapshots/</url>
        </snapshotRepository>
    </distributionManagement>
  ...
</project>
```
2. 配置好插件，然后 mvn deploy
```shell
mvn deploy
```
# 进阶用法
打包源码jar包
```shell
mvn source:jar
```
打包源码 测试 jar包
```shell
mvn source:test-jar
```