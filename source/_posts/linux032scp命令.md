---
title: scp命令
date: 2022-01-08 12:57:43
tags:
categories: linux
doc:
---

### 超级复制

​	使用该命令的前提条件是目标主机已经成功安装openssh-server
​	

### 使用格式：

​	

```
scp -r 目标用户名@目标主机ip地址:/目标文件的绝对路径 /保存到本机的绝对（相对）路径
```

​	拷贝目录要加-r，拷贝文件不需要加-r
​	

```
scp -r yxc19980620c@192.168.44.135:/home/yxc19980620c/test2 ./test3
```
