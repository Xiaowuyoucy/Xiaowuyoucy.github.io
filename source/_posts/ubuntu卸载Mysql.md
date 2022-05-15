---
title: ubuntu卸载Mysql
date: 2022-05-15 09:43:33
tags:
categories: linux
doc:
---

## 查看mysql依赖

```xml
dpkg --list|grep mysql
```

## 卸载 mysql-common

```xml
sudo apt-get remove mysql-common
sudo apt-get autoremove --purge mysql-server-5.0
```

## 查看是否剩下mysql+清除残留数据

```xml
dpkg --list|grep mysql
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P 
```