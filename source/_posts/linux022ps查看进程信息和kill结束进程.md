---
title: ps查看进程信息和kill结束进程
date: 2022-01-08 00:16:08
tags:
categories: linux
doc:
---

## ps查看进程信息

​	参数：	  
​		  a   显示现行终端机下的所有程序，包括其他用户的程序。
​		  u代表 以用户为主的格式来显示程序状况。
​		  x 代表没有终端的程序

```
ps aux
```

<br/><br/>

### | 管道

​	将A输出作为B输入

```
ps aux | grep bash  //将ps输出的结果作为grep的输入，然后将结果显示回终端
```

<br/><br/><br/>

## kill

`kill -l   //查看信号`

`kill -信号(可以只写数字) PID`

```
kill -9 13442
kill -SIGKILL 123432
```

