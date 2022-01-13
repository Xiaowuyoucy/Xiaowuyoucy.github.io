---
title: 并行和并发的概念
date: 2022-01-12 02:48:13
tags:
categories: linux
doc:
---



并发，在一个时间段内, 是在同一个cpu上, 同时运行多个程序。

如：若将CPU的1S的时间分成1000个时间片，每个进程执行完一个时间片必须无条件让出CPU的使用权，这样1S中就可以执行1000个进程。

![img](/images/javawz/wpsE977.tmp.jpg) 

![img](/images/javawz/wpsE978.tmp.jpg) 

![img](/images/javawz/wpsE989.tmp.jpg) 

![img](/images/javawz/wpsE98A.tmp.jpg) 

并行性指两个或两个以上的程序在同一时刻发生(需要有多颗)。

![img](/images/javawz/wpsE98B.tmp.jpg) 

![img](/images/javawz/wpsE98C.tmp.jpg)





​	并发: 在一个时间段内, 一个CPU上, 有多个程序在执行.
​	并行: 在一个时间片内, 有多个程序在执行(前提是有多个cpu)
​	cpu会将一个大的时间段分成多个小的时间片, 让进程轮流使用CPU的时间片.