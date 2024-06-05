---
title: npm换源
categories:
  - 开发
tags:
  - npm
date: 2020-12-28 00:04:14.357000
---
[toc]
# 一些镜像地址：

## 淘宝npm镜像
```
搜索地址：http://npm.taobao.org/
registry地址：http://registry.npm.taobao.org/
```
## cnpmjs镜像
```
搜索地址：http://cnpmjs.org/
registry地址：http://r.cnpmjs.org/
```
# 换源方法
- npm临时换源
```
npm install express --registry=https://registry.npm.taobao.org
```

- 持久使用
```
npm config set registry https://registry.npm.taobao.org
// 配置后可通过下面方式来验证是否成功
npm config get registry
// 或npm info express
```

- 通过cnpm
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
// 使用cnpm install expresstall express
```