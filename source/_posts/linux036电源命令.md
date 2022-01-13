---
title: 电源命令
date: 2022-01-08 13:04:41
tags:
categories: linux
doc:
---

#### poweroff 关机

```
poweroff
```



#### reboot 重启

```
reboot
```



#### shutdown

```
	-a 	指定权限
	-r 	重启计算器（和reboot）命令一样
	-k 	模拟关机（只向用户发出警告信息，但不关机）
	-h 	关闭计算机并关闭电源（常用）
	-n 	不调用init进程关闭计算机（不推荐）
	-c 	取消正在执行的关机命令
	-f 	重启计算机，但不进行磁盘检测
	-F 	重启计算机，进行磁盘检测
	-t(秒) 	指定发出警告信息与删除信息时要延迟的秒数
```



```
shutdown -r now 	//加now则是立即重启
shutdown -h +1 "1 minute after shutdown"		//设置1分钟以后关闭计算机，并在SSH中提示“1 minute after shutdown”

shutdown -c 					//取消关机
shutdown now					//切换至单人操作模式
```



