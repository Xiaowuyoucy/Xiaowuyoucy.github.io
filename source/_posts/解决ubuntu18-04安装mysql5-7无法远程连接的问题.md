---
title: 解决ubuntu18.04安装mysql5.7无法远程连接的问题
date: 2022-05-15 09:48:31
tags:
categories: 数据库
doc:
---

遇到mysql本地连接成功,但远程连接不上的问题，无非从以下几个方面入手：

## 1、user表问题

mysql库中user表的root用户的host没有指定为%，而是默认的localhost，在mysql客户端执行如下操作即可

```
# 1、切换到mysql库
use mysql;
#2、查看root用户的host是不是 %,如果是localhost继续下面操作
select user,authentication_string,host from user where user = 'root';
# %为任意主机，就是任意主机可以通过用户名root + 密码 进行连接，你也可以指定只允许某IP的主机进行连接
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '密码';
# 如果之前没有密码，最好再设置下密码，发现上面语句不会产生密码，否则后面连接可能会提示无权限需要更新客户端
alter user 'root'@'%' identified with mysql_native_password by '密码';
# mysql重新加载用户权限
flush privileges;
```

### 2、防火墙问题

如果上面操作之后仍然无法远程连接mysql，就需要检查防火墙问题。我这里是ubuntu系统（其他系统自己找对应的指令）
 首先查看了防火墙是否开启：

```
# 查看防火墙状态
ufw status
```

发现并没有开启，因此排除防火墙问题。



### 3、3306端口开放的问题

如果3306端口没有开发也会导致无法远程连接，通过指令发现3306开放了。

```
    netstat -tunlp
```

![解决ubuntu18.04安装mysql5.7无法远程连接的问题](/images/javawz/2021093001031440.png)

 但是仔细看图，有一点奇怪，就是它开放的是`127.0.0.1:3306`而不是`0.0.0.0:3306`，这才是本次问题的突破点，`127.0.0.1:3306`只对本机开放访问，其他ip都不行，那么该怎么解决呢？重点来了~
 1）由于是apt在线安装的，mysql在目录`/etc/mysql/mysql.conf.d`下有个默认配置文件`mysqld.cnf`，打开编辑发现：

![解决ubuntu18.04安装mysql5.7无法远程连接的问题](/images/javawz/2021093001095056.png)

2）原来罪魁祸首就在这里，`bind-address`绑定的ip是`127.0.0.1`，我们只需将`127.0.0.1`改为`0.0.0.0`表示任意ip主机都能访问即可：

![解决ubuntu18.04安装mysql5.7无法远程连接的问题](/images/javawz/2021093001120039.png)

3）最后，重启mysql服务即可。