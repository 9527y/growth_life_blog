---
title: kubernetes coredns无法解析私有harbor域名问题处理
categories:
  - 运维
tags:
  - k8s
  - coredns
date: 2021-06-03 08:29:14
---
修改coredns配置，增加host对应；

```
1,/etc/hosts 添加相关的域名解析
xx.xx.xx.xx harbor.alexdev.com
 
2，修改coredns configmap，添加hosts语段：
# kubectl get cm coredns -n kube-system -o yaml
apiVersion: v1
data:
  Corefile: |
    .:53 {
        errors
        health {
            lameduck 5s
        }
        ready
        kubernetes cluster.local in-addr.arpa ip6.arpa {
            pods insecure
            fallthrough in-addr.arpa ip6.arpa
        }
        ## 主要为以下内容(注意末尾添加 .cluster.local)：
        ## hosts内部可以添加多个域名
        hosts {
          xx.xx.xx.xx harbor.alexdev.com.cluster.local
          fallthrough
        }
        prometheus :9153
        forward . /etc/resolv.conf {
            prefer_udp
        }
 
3,添加后，如果pod内仍无法ping通域名，可以删除coredns pod，重新加载cm文件
# kubectl get pods -A|grep coredns
kube-system                    coredns-6b55b6764d-7wdsq                                          1/1     Running     1          18h
kube-system                    coredns-6b55b6764d-d4q72                                          1/1     Running     1          18h
# kubectl delete pod coredns-6b55b6764d-7wdsq -n kube-system
# kubectl delete pod coredns-6b55b6764d-d4q72 -n kube-system
 
4,测试
kubectl run -i --tty --image busybox:1.28.4 dns-test --restart=Never --rm /bin/sh
 
$ nslookup harbor.alexdev.com.svc (域名后加service名字)
```