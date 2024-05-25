---
title: 使用Docker在内网搭建DNS服务器
categories:
  - 运维
tags:
  - usage
  - docker 
date: 2020-05-26 18:22:39.925000
---
# dnsmasq
# 搭建web配置界面
使用[jpillora](https://github.com/jpillora/docker-dnsmasq)提供的docker镜像。
## 用法
创建一个配置文件`/opt/dnsmasq.conf`，如下为demo
```
#dnsmasq config, for a complete example, see:
#  http://oss.segetech.com/intra/srv/dnsmasq.conf
#log all dns queries
log-queries
#dont use hosts nameservers
no-resolv
#use cloudflare as default nameservers, prefer 1^4
server=1.0.0.1
server=1.1.1.1
strict-order
#serve all .company queries using a specific nameserver
server=/company/10.0.0.1
#explicitly define host-ip mappings
address=/myhost.company/10.0.0.2
```
## 启动容器

```
$ docker run \
	--name dnsmasq \
	-d \
	-p 53:53/udp \
	-p 5380:8080 \
	-v /opt/dnsmasq.conf:/etc/dnsmasq.conf \
	--log-opt "max-size=100m" \
	-e "HTTP_USER=foo" \
	-e "HTTP_PASS=bar" \
	--restart always \
	jpillora/dnsmasq
```
其中：`	-e "HTTP_USER=foo" 	-e "HTTP_PASS=bar" ` 为为dnsmasq设定访问用户名和密码。访问地址：`

# 指定默认的dns服务器
默认的dns服务器
```
server=1.0.0.1
server=1.1.1.1
```
# 为某个域名使用指定dns
例子：为example.com使用Google的dns：
```
server=/example.com/8.8.8.8
```
# 设定域名解析
例子：将myhost.company解析到10.0.0.2
```
address=/myhost.company/10.0.0.2
```


