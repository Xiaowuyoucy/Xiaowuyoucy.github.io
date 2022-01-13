---
title: ftp服务器配置
date: 2022-01-08 00:40:15
tags:
categories: linux
doc:
---

ftp服务器名:`vsftpd`

作用:文件上传和下载

<br /><br />

#### ftp服务器配置

​	配置文件在`/etc/vsftpd.conf`

```
anonymous_enable=YES  允许匿名用户登录

write_enable=YES   实名登录用户拥有写的权限

local_umask=022 设置本地掩码为022



anon_mkdir_write_enable=YES  匿名用户可以在ftp服务器上创建目录

listen_ipv6=NO

listen=YES
```

<br /><br />

#### 重启ftp服务

```
sudo service vsftpd restart
```

<br /><br />

#### ftp服务

```
sudo service vsftpd start 启动ftp

sudo service vsftpd stop关闭ftp服务

sudo service vsftpd restart重启ftp服务。

service vsftpd status查看状态
```

<br /><br />

#### 实名登录ftp

```
ftp ip地址
	输入用户
	输入密码
```


​	<br /><br />

#### 退出

```
bye 
quit
exit
```



<br /><br />

#### 文件上传和下载

上传 
	在什么目录登录的,文件默认从那里找
	`put 文件名`

```
put aaa
```

下载

`get 文件名`

```
get aaa
```

不允许操作目录,可以打包处理

<br /><br />

#### 匿名登录服务器

用户名是`anonymous`

密码是空
	不允许匿名用户在任意目录直接切换
	只能在一个指定的目录范围内工作
	需要在ftp服务器上创建一个匿名用户的目录 --匿名用户的根目录

配置文件在`/etc/vsftpd.conf`
	在配置文件添加 ` anon_root=/home/yxc19980620c/myFtp/`
	匿名登录默认路径是在`/srv/ftp`
	在配置文件`/etc/passwd`可以查看并修改

```
ftp:x :123:127:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin
```

<br /><br /><br /><br />