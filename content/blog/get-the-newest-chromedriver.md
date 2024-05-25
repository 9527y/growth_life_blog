---
title: 获取最新的GoogleDriver，下载指定版本
categories:
  - 运维
tags:
  - null
date: 2024-05-18 09:19:40
---

对于114之前的版本参考：
https://developer.chrome.com/docs/chromedriver/downloads

对于114之后的版本访问：
## 下载最新版本

获取地址：https://googlechromelabs.github.io/chrome-for-testing

## 下载制定版本
可能chrome的版本不是最新的，所以需要指定版本。在最新版本中得到地址：`https://storage.googleapis.com/chrome-for-testing-public/125.0.6422.60/mac-arm64/chromedriver-mac-arm64.zip`

在url中有一段是版本号，根据你的chrome版本，替换那个版本号，比如我用的是`124.0.6367.203`。所以得到的下载地址是：

`https://storage.googleapis.com/chrome-for-testing-public/125.0.6422.60/mac-arm64/chromedriver-mac-arm64.zip`
