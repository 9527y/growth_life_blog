---
title: MAVEN发布项目到私服，镜像
categories:
  - 开发
tags:
  - maven
date: 2020-05-31 17:51:24.420000
---

# snapshot和release的区别
- snapshot快照库，开发过程中随时发布，`无需更改版本号，适用不稳定的开发阶段`
- release，版本稳定后，可以发布到此库。
- 快照版本后缀`-SNAPSHOT`，发布到不同的库完全是依靠版本号确定，程序自动判断。

快照版本示例
```xml
	<groupId>com.alexdev</groupId>
	<artifactId>mm</artifactId>
	<version>0.1-SNAPSHOT</version>
	<packaging>jar</packaging>
```
# 发布到私服
一般私服使用Nexus SSO搭建。在官网下载安装包安装即可（ https://www.sonatype.com/nexus-repository-oss ）。目前版本：Nexus Repository Manager OSS 3.x （2019-01-08）
默认密码：`admin`/`admin123`
安装完后设置maven的setting文件：
```xml
  <server>
    <id>releases</id>
    <username>admin</username>
    <password>admin123</password>
  </server>
  <server>
    <id>snapshots</id>
    <username>admin</username>
    <password>admin123</password>
  </server>
```
在如上配置设置了两个账户密码，这个账户密码是nexus的账户密码，如需新建，请与之对应。
之后设置项目的pom文件，注意id和settting.xml文件中的id需相对应。代表发布到不同的代码库中的不同账户和密码。对于url地址，不同的版本可能不同。
```xml
<!-- 项目发布管理 -->
<distributionManagement>
  <repository>
    <id>releases</id>
    <name>User Project Release</name>
    <url>http://192.168.1.53:8081/repository/maven-releases/</url>
  </repository>

  <snapshotRepository>
    <id>snapshots</id>
    <name>User Project SNAPSHOTS</name>
    <url>http://192.168.1.53:8081/repository/maven-snapshots/</url>
  </snapshotRepository>
</distributionManagement>
```
![url地址的查看](https://img-blog.csdnimg.cn/20190108103909237.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hheHllaw==,size_16,color_FFFFFF,t_70)

发布到私服的命令：
```shell
mvn clean deploy -X -Dmaven.test.skip=true
```
如果看到`BUILD SUCCESS`字样，既可在仓库里查看相应的包。

# Tips
- 如果出现400，需要注意项目下的pom.xml文件和maven使用的setting.xml文件的配置是否一致。
- 如果出现401，需要检查maven使用的setting.xml中的帐号和密码是否正确，相应的repository是否为“Allow Redeploy”。
# 镜像mirror
镜像的目的是为了解决访问速度问题，个人开发可以设置国内镜像，例如阿里云提供的的，在setting.xml文件中添加
```xml
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
```
如果是团队开发，需要设置上文中所安装的私服release镜像。具体url查看如上文图中所示。
```xml
    <mirror>
      <id>InterNet</id>
      <name>InterNet maven</name>
      <url>http://192.168.1.53:8081/repository/maven-public</url>
      <mirrorOf>central</mirrorOf>
    </mirror>
```
mirror的配置只针对release包，对于snapshot镜像采用如下配置setting.xml：
```xml
<profiles>
    <profile>
      <id>nexus</id>
      <repositories>
        <repository>
          <id>nexus_snapshot_repository</id>
          <releases>
            <enabled>false</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
          <url>http://192.168.1.53:8081/repository/maven-snapshots/</url>
          <layout>default</layout>
        </repository>
        <repository>
          <id>central</id>
          <url>http://192.168.1.53:8081/repository/maven-public</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </repository>
      </repositories>
      <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>http://192.168.1.53:8081/repository/maven-public</url>
          <releases>
            <enabled>true</enabled>
          </releases>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
          <updatePolicy>always</updatePolicy>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>

  <activeProfiles>
    <activeProfile>nexus</activeProfile>
  </activeProfiles>
```
以上配置根据实际情况修改。

通过updatePolicy控制更新（默认值是daily，表示Maven每天检查一次。其他可用的值包括：
- never-从不检查更新；
- always-每次构建都检查更新；
- interval：X-每隔X分钟检查一次更新（X为任意整数）