---
title: MYSQL服务端的登录和退出
date: 2020-02-27 06:49:57
tags:
categories: MYSQL
doc:
---

# MYSQL服务端的登录和退出

1.用`mysql`自带的客户端

![1582757475054](/images/javawz/1582757475054.png)

直接输入密码就可以进入数据库管理系统了

2.使用`cmd`登录

```
//   -h 连接的主机  -P 代表端口号

//第一种方式

mysql -h 主机名 -P 端口号 -u 用户名 -p

// mysql -h localhost -P 3306 -u root -p


//第二种方式
mysql -h 主机名 -P 端口号 -u 用户名 -p密码
//mysql -h localhost -P 3306 -u root -p123
//-p后面没有空格


//第三种方式
//在服务端本机登录可以省略
mysql -u 用户名 -p

mysql -u 用户名 -p密码
```

### 退出

用`exit`命令或者`Ctrl + C`