---
title: CentOS7 firewall 开放端口和关闭防火墙
categories:
  - 运维
tags:
  - firewalld
  - centos7
date: 2020-05-31 17:30:41.909000
---
CentOS7开放端口，重载防火墙配置

```
sudo firewall-cmd --zone=public --add-port=6379/tcp --permanent
sudo firewall-cmd --reload
```

检查防火墙规则命令

```
firewall-cmd --list-all
```

会显示：

```
public (active)   # 状态
  target: default
  icmp-block-inversion: no
  interfaces: ens33    #接口信息
  sources: 
  services: ssh dhcpv6-client    #开放服务
  ports: 6379/tcp    # 开放端口
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```

其他命令：
```
# 重启防火墙
systemctl restart firewalld  
# 检查状态
firewall-cmd --state
firewall-cmd --list-all
#禁用 firewall
systemctl disable firewalld
systemctl stop firewalld
# 查询防火墙状态
systemctl status firewalld
#启用防火墙
systemctl enable firewalld
systemctl start firewalld
# 查询防火墙状态
systemctl status firewalld
```