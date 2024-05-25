---
title: FireFox和Chrome浏览器Cookies存储路径记录
categories:
  - 杂记
tags:
  - cookie
date: 2021-03-20 17:55:11
---

## Firefox

- Windows 

Cookie数据位于：%APPDATA%\Mozilla\Firefox\Profiles\ 目录中的xxx.default目录，名为cookies.sqlite的文件，如
```
C:\Users\xxx\AppData\Roaming\Mozilla\Firefox\Profiles\hsi4acx.default\cookies.sqlite
```
- Mac

官网


> Finding your profile without opening Firefox
> Click the Finder icon in the dock. On the menu bar, click the Go menu, hold down the option or alt key and select Library. A window will open containing your Library folder.
> Open the Application Support folder, then open the Firefox folder, and then the Profiles folder.
> Your profile folder is within this folder. If you only have one profile, its folder would have "default" in the name.

找到 default结尾的目录

```
/Users/xxx/Library/Application Support/Firefox/Profiles
```

## Chrome

- windows

```
C:\Users\%USERNAME%\AppData\Local\Google\Chrome\UserData\Default\
```

- mac

```
~/Library/Application Support/Google/Chrome
```

- linux

```
~/.config/google-chrome

```
