---
title: Docker拉取oracle 11g镜像配置的详细教程
date: 2022-05-13 06:22:30
tags:
categories: linux
doc:
---

#### docker拉取oracle数据库镜像

```
docker search oracle
```

查看可以使用docker拉取的oracle镜像；我是选择的Oracle12c这一款，truevoly/oracle-12c是复制之前的sath89/oracle-12c image镜像。

![在这里插入图片描述](/images/javawz/5a025e6dda1a45f79592f8b8575a17c7.png)

### 开始拉取镜像-执行命令：

```
docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```

下载完成后 查看镜像：

```
docker images
```

![img](/images/javawz/2021091810223023.png)





### 创建容器

```
docker run -d -p 1521:1521 --name oracle11g registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
```

***参数 -d 后台运行容器，并返回容器ID ; -p 指定端口 , -v 指定数据卷位置*** 

***数据卷的特点：
 1、可以供多个容器访问，直接共享或者重用
 2、独立于容器周期，不会在删除容器时删除其挂载的数据卷
 3、数据卷在容器启动时初始化，如果容器使用的镜像在挂载点包含的数据这些数据会复制到新的容器中
 4、可以直接对数据卷里的内容进行修改***



这里说一下，命令后面的地址一定要是你下载的镜像地址也就是你拉取镜像名字，否则会出现名字已存在等问题！

​    如果创建成功能会返回容器id

```
ee92e88fea086c05af307d0274e1555ab04bfe747bb7b5b5e2198074bc40cdde
```



### 启动容器

```
docker start oracle11g
```

![img](/images/javawz/2021091810223024.png)

### 进入镜像进行配置

```
docker exec -it oracle11g bash
```



### 创建软连接

```
ln -s $ORACLE_HOME/bin/sqlplus /usr/bin
```



```
//ctrl + p + q 退出容器（注意，不要exit退出，防止容器直接关闭了）
```



### 查看容器运行状态

```
docker ps -a
```





```
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE                 COMMAND             CREATED         STATUS         PORTS                                                                                                                         NAMES
ee92e88fea08   truevoly/oracle-12c   "/entrypoint.sh "   6 minutes ago   Up 6 minutes   0.0.0.0:1521->1521/tcp, :::1521->1521/tcp, 0.0.0.0:2122->22/tcp, :::2122->22/tcp, 0.0.0.0:9090->8080/tcp, :::9090->8080/tcp   oracle12c
//进入容器内部
[root@localhost ~]# docker exec -it ee92e88fea08 /bin/bash
//切换成oracle用户
root@ee92e88fea08:/# su oracle
//进入sqlplus
oracle@ee92e88fea08:/$ $ORACLE_HOME/bin/sqlplus / as sysdba

SQL*Plus: Release 12.1.0.2.0 Production on Sun Aug 1 03:15:37 2021

Copyright (c) 1982, 2014, Oracle.  All rights reserved.


Connected to:
Oracle Database 12c Standard Edition Release 12.1.0.2.0 - 64bit Production
//设置密码有效期为无限制
SQL> ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;

Profile altered.
//解锁system用户
SQL> alter user SYSTEM account unlock;

User altered.
//创建一个账号为test_lx的用户密码设置为test_lx
SQL> create user test_lx identified by test_lx;     

User created.
//为这个用户赋予管理员的权限
SQL> grant dba to test_lx;

Grant succeeded.

SQL> read escape sequence

//ctrl + p + q 退出容器（注意，不要exit退出，防止容器直接关闭了）
```

