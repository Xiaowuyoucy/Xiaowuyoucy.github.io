---
title: 文件和目录属性命令
date: 2022-01-07 23:28:35
tags:
categories: linux
doc:
---

wc命令、od命令、du命令、df命令、which命令、whoami命令

<br /><br />

## 	wc 获取文本文件的信息

```
yxsdc19c@yscc:/usr/include$ wc time.h
  309  1515 10360 time.h
```

309代表行数 1515代表单词个数 10360代表字节数

<br /><br />

## od 查看二进制文件

```
od -t 文件名
```

```
-t 指定数据的显示格式
-tc ASCII字符
-tx 十六进制数
-td 有符号十进制数
-tu 无符号十进制数
-to 八进制数
-tf 浮点数
```

<br /><br />

## 查看某个目录的大小du

```
du -h 目录
```

-h代表人性化输出

<br /><br />

## df查看磁盘的使用情况

```
df -h
```

<br /><br />

## 查看命令在哪个目录 which

```
which 命令名
```

​	内建命令是查不到的，例如cd，只能查外建命令

<br /><br />

## 查看当前用户 whoami

```
whoami
```

<br /><br />

