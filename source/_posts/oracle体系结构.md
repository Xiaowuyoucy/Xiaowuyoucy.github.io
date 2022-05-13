---
title: oracle体系结构
date: 2022-05-09 20:36:53
tags:
categories: 数据库
doc:
---

## Oracle的体系结构

Oracle服务器：是一个数据管理系统(RDBMS)，它提供开放的, 全面的, 近乎完整的信息管理。由1个数据库和一个（或多个）实例组成。数据库位于硬盘上，实例位于内存中。

![img](/images/javawz/wps939A.tmp.jpg)



PGA(程序全局区): 处理客户端请求

SGA(系统全局区): 数据提交后SGA的内容会写到磁盘中,提交之后不能回滚,SGA比较吃内存



### 表空间(users)和数据文件

逻辑概念：表空间，表空间由多个数据文件组成。位于实例上，在内存中。

物理概念：数据文件，位于硬盘之上。(C:\app\Administrator\oradata\orcl目录内后缀为.DBF的文件)

​		 一个表空间可以包含一个或者是多个数据文件。1：n（表空间：数据文件）



### 段、区、块

![img](/images/javawz/wps19AE.tmp.jpg)



 段存在于表空间中； 段是区的集合； 区是数据块的集合； 数据块会被映射到磁盘块。



![img](/images/javawz/wps538B.tmp.jpg)



![img](/images/javawz/wps6E77.tmp.jpg)





### 查看oracle实例服务

```
ps -ef | grep 实例名_
ps -ef | grep orcl_
```



### UPS不间断电源供电系统

如果一个电源断电了,UPS系统马上启动,并立马供电,所以不会出现断电



如果一个数据库中有多个实例可以起到负载均衡的作用.

比如同时有1万个用户操作数据库,那么实例A可以接收5000个用户,实例B可以接收5000个用户,这样就不会出现一个实例卡死