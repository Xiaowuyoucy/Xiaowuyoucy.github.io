---
title: nfs服务器
date: 2022-01-08 00:56:00
tags:
categories: linux
doc:
---

### 安装	

```
sudo apt-get install nfs-kernel-server
```

功能：	创建一个共享目录


​	

1).先创建一个目录 

```
mkdir 目录名
```

2).修改配置文件
	`/etc/exports`
	添加这一行	`/home/用户名/共享目录 *（访问权限，sync）`
		

```
/home/qwe/aa *（rw，sync）
*代表ip地址	访问权限有ro（只读），rw（可读可写），sync（代表实时更新）
ro		只读
rw		读写
root_squash	当NFS客户端以root管理员访问时，映射为NFS服务器的匿名用户
no_root_squash	当NFS客户端以root管理员访问时，映射为NFS服务器的root管理员
all_squash		无论NFS客户端使用什么账户访问，均映射为NFS服务器的匿名用户
sync		同时将数据写入到内存与硬盘中，保证不丢失数据
async		优先将数据保存到内存，然后再写入硬盘；这样效率更高，但可能会丢失数据
```



#### 重启服务

```
sudo service nfs-kernel-server restart
```

#### 客户端：

​	挂载服务器共享目录

`mount 服务器ip:共享目录   /mnt`

```
mount 192.168.1.123:/home/xiaoh/nfs /mnt
```



