---
title: ssh 公钥认证方式登录（免密登录）
categories:
  - 运维
tags:
  - centos7
date: 2020-07-01 22:41:20.738000
---
一般情况下,我们用ssh远程登录到服务器时，要输入用户名和密码。这对经常维护系统的人来说，很麻烦。怎样才能不用密码直接登录到远程的linux/unix服务器呢？ssh公钥认证可以解决这个问题。

公钥认证，是使用一对加密字符串，一个称为公钥(public key)， 任何人都可以看到其内容，用于加密；另一个称为密钥(private key)，只有拥有者才能看到，用于解密。 通过公钥加密过的密文使用密钥可以轻松解密，但根据公钥来猜测密钥却十分困难。

## 服务器端
在使用公钥认证之前，先检查一下服务器的ssh配置文件/etc/ssh/sshd_config

```
RSAAuthentication yes        # 启用 RSA 认证，默认为yes
PubkeyAuthentication yes     # 启用公钥认证，默认为yes
```

如果配置没有问题，那么你就可以进行下一步了。

## 例子
下面我们举个例子，比如有两台机器，客户机A与服务器B，想用ssh公钥认证方式从A机器用client用户登录到B机器的server用户，方法如下：

1. 在客户机A上生成公钥与密钥

```
[client@test ~]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/client/.ssh/id_rsa): #此处直接按回车即可
Created directory '/home/client/.ssh'.
Enter passphrase (empty for no passphrase): #此处直接按回车即可
Enter same passphrase again: #此处直接按回车即可
Your identification has been saved in /home/client/.ssh/id_rsa.
Your public key has been saved in /home/client/.ssh/id_rsa.pub.
The key fingerprint is:
f5:30:ba:10:ee:7a:c6:cf:d8:ec:3f:4c:b3:f1:09:6d client@linuxsong.org

```
这样就生成了client用户在这台机器的公钥(`/home/client/.ssh/id_rsa.pub`)和私钥(`/home/client/.ssh/id_rsa`).


2. 将上一步生成的公钥文件拷贝到服务器B上。然后将文件内容追加到server用户目录的.ssh/authorized_keys中：

```
[server@server]$ cat id_rsa.pub >> .ssh/authorized_keys
```

这样，client用户从客户机上登录到服务器的server用户，就不用再输入密码了。

另外，如果对服务器安全性比较高的情况下，可以设置用户只允许通过公钥认证，禁止用户用密码方式登录，只要修改一下服务器的配置文件`/etc/sshd/sshd_config`

```
PasswordAuthentication no
```

修改完后要重启sshd服务。
这样用户通过密码方式登录时就会提示：
```
Permission denied (publickey,gssapi-with-mic)
```

有效的提高了系统的安全性。

## 注意：
- `.ssh` 目录的权限必须是0700
- `.ssh/authorized_keys` 文件权限必须是0600，否则公钥认证不会生效。