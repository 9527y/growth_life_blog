---
title: es设置开机自启动
categories:
  - 开发
tags:
  - elaticsearch
date: 2020-10-22 11:18:53.873000
---
[toc]
## 新建脚本

```
vi /etc/init.d/es
```
填入如下内容：

```
#!/bin/sh
#chkconfig: 2345 80 05
#description: elasticsearch

#jdk相关路径
export JAVA_HOME=/var/local/java/jdk1.8.0_241
export JAVA_BIN=/var/local/java/jdk1.8.0_241/bin
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME JAVA_BIN PATH CLASSPATH

case "$1" in
start)
    #es的启动账号名
    su es<<!
    #es的安装路径
    cd /data/elasticsearch-6.8.11/
    ./bin/elasticsearch -d
!
    echo "elasticsearch startup"
    ;;
stop)
    es_pid=`ps aux|grep elasticsearch | grep -v 'grep elasticsearch' | awk '{print $2}'`
    kill -9 $es_pid
    echo "elasticsearch stopped"
    ;;
restart)
    es_pid=`ps aux|grep elasticsearch | grep -v 'grep elasticsearch' | awk '{print $2}'`
    kill -9 $es_pid
    echo "elasticsearch stopped"
    su es<<!
    cd /data/elasticsearch-6.8.11/
    ./bin/elasticsearch -d
!
    echo "elasticsearch startup"
    ;;
*)
    echo "start|stop|restart"
    ;;
esac

exit $?

```
`es`为启动elasticsearch的用户；

## 设置权限及后续操作

```
chmod +x /etc/init.d/es
chkconfig --add es
```