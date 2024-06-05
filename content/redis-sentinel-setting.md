---
title: redis哨兵机制
categories:
  - 运维
tags:
  - redis
date: 2020-07-31 17:14:56.542000
---
# 背景
我司使用仅有两台redis实例，所以不太适合做redis群集，比较适合使用redis哨兵机制。

## 原理
监控master故障时，使用投票机制（sdown，odown），判定是否故障，移除；选举新的slave作为master节点。`redis.conf`会随之变化。

## 哨兵机制关键配置
```
# 哨兵sentinel监控的redis主节点的 ip port
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 172.16.8.206 6379 1

# Default is 3 minutes.
指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒。
sentinel down-after-milliseconds mymaster 3000

```
## 启动

```
./redis-sentinel sentinel.conf
```

## 注意
- 启动顺序：master->slave->sentinel
- master如果down掉，再启动不会他的角色是slave