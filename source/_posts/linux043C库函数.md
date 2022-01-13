---
title: C库函数
date: 2022-01-08 17:19:07
tags:
categories: linux
doc:
---



![image-20220108171913561](/images/javawz/image-20220108171913561.png)

<br/><br/>

FILE是一个结构的,有三个成员,

第一个是文件描述符(整型)用来保存索引

第二个是文件读写指针位置

第三个是I/O缓冲区,默认缓冲区大写是8KB