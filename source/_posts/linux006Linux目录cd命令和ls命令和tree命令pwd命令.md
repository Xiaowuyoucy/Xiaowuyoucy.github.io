---
title: Linux目录cd命令和ls命令和tree命令pwd命令
date: 2022-01-07 22:52:18
tags:
categories: linux
doc:
---

## 目录

/ 根目录
ls /

/bin  常用的命令

cd /bin  进入bin目录

/boot 开机启动项文件

/dev  设备文件

/etc 配置文件

/home 主目录,存放用户

/lib  动态链接库(共享库)

/lost-found  存放文件碎片

/media  挂载外设 U盘或光盘等等

/mnt  手动挂载外设到这个目录

/opt 第三方软件

/root 超级用户目录

/sbin 管理员使用的系统管理程序

/usr 用户软件资源目录(用户的软件或文件)

/usr/bin 系统用户的应用程序

/usr/sbin 超级用户使用的管理程序和系统守护程序

/usr/src 内核源码默认的放置目录

<br><br><br><br>





## cd命令

cd 进入指定目录

绝对路径 从/开始
相对路径
	./当前路径
	../上一级路径
	cd -  两个相邻目录切换
	cd ~  切换到当前用户目录
	cd  切换到用户目录



yxc19980620c@yc:~$

yxc19980620c  用户名
yc 主机名
~ 用户目录
$普通用户

超级用户root   切换到超级用户 sudo su  或 su root





## ls命令

-rw-r--r--  1 root root   1937 9月   2  2020 ucontext.h

文件类型 所有者 所属组 其他人 硬连接数 文件所有者 文件所属组 文件大小 日期 文件名

文件类型

 - 普通文件
   d 目录
   l 链接符号
   b 块设备
   c 字符设备
   s socket文件
   p 管道 


ls和tree的使用:

ls -a 显示所有文件
ls -l 详细显示
ls -R 递归列出当前目录
ls -al
ls -h 人性化显示文件大小

`ls -l` 也可以写成`ll`

## tree命令和pwd命令

`tree 目录`

`pwd `  查看当前位置

