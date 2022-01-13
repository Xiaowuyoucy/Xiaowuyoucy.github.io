---
title: 用户管理
date: 2022-01-08 00:29:32
tags:
categories: linux
doc:
---

## 创建用户

### adduser

`adduser 用户名`

创建一个用户
​	需要管理员权限
​	用户名不能有大写字母

```
sudo adduser abc
```

<br /><br />

### useradd

​	创建一个用户
​	用户名可以大写
​	需要管理员权限

```
useradd -s /bin/bash -g xiaoai -d /home/xiaoai -m xiaoai
```

​	-s 表示使用什么命令解析器
​	-g 用户组
​	-d 用户家目录
​	-m 如果用户家目录不存在,则创建一个目录

<br />

<br />

<br />



## 添加用户组

### groupadd

​	添加一个用户组
​	需要管理员权限
​	`groupadd 用户组名`

```
groupadd abc
```

​	<br /><br /><br />

## 修改用户密码

### passwd

​	修改用户密码
​	需要管理员权限

`passwd 用户名`

```
passwd root

passwd 			//修改当前用户密码

sudo passwd		//修改root用户密码
```

​	passwd 不加sudo 不输入用户名表示修改当前用户密码
​	sudo passwd 表示修改root用户密码

<br /><br /><br />

## 删除用户

### deluser 

​	删除用户
​	`deluser 用户名`

```
deluser aaa
```

​	需要手动删除家目录

### userdel

​	删除用户
​	`userdel -r 用户名`

```
userdel -r abc
```

​	不需要手动删除家目录

<br /><br /><br />

## 查看当前系统下有哪些用户

```
vim /etc/passwd
yxc19980620c:x:1000:0:yc:/home/yxc19980620c:/bin/bash
```

​	/bin/bash 代表使用什么命令解析器

<br /><br /><br />

## 切换用户

### su命令

`su 用户名`

```
su root
```

<br />

<br />

<br />

## 退出当前用户

```
exit 
```

​	