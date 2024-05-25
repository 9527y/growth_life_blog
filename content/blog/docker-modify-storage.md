---
title: docker修改存储位置
categories:
  - 运维
tags:
  - docker
date: 2023-06-05 12:52:58
---
[toc]

## 背景
因为随着容器启动越来越多，存储占据越来越多的空间：`/var/lib/docker/overlay2/`，所以修改挂载位置，可以是外部空间等。

## 修改存储位置


1. 停止服务

```shell 
systemctl stop docker 
```

2. 将存储路径复制到新目录下 <新目录>

```shell
mkdir -p <新目录>
sudo rsync -aqxP /var/lib/docker/ <新目录>
```

3. 修改启动脚本`/lib/systemd/system/docker.service`。

```
# 将
[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

# 修改为
ExecStart=/usr/bin/dockerd -g <新目录> -H fd:// --containerd=/run/containerd/containerd.sock

```

4. 重启服务

```shell
systemctl daemon-reload;
systemctl start docker;
```

5. 之前的目录可以清理了。