---
title: C库函数与系统函数的关系
date: 2022-01-08 18:52:02
tags:
categories: linux
doc:
---

![image-20220108185220142](/images/javawz/image-20220108185220142.png)

### printf是如何显示消息的

首先printf函数有一个stdout指针

stdout是FILE*指针

priintf会调用系统应用层wirte函数,然后wirte函数会从用户空间转换为内核空间，然后调用sys_wirte()函数(系统调用)，接着sys_write()会调用内核层的设备驱动函数,通过设备驱动操作硬件显示字符串