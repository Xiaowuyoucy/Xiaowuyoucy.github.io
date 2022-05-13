---
title: sqlplus的基本操作
date: 2022-05-11 19:20:29
tags:
categories: 数据库
doc:
---



### 	显示当前用户:

```
show user
```



### 	查看当前用户下的表:

```
select * from tab;
```



### 	查看表结构:

```
desc 表名;
```



### 	设置行宽:

```
set linesize 140 
```



### 	设置页面显示的行数:

```
set pagesize 100
```



### 永久设置行宽和页面显示的行数:

```
C:\app\Administrator\product\11.2.0\client_1\sqlplus\admin\glogin.sql 
C:\app\Administrator\product\11.2.0\dbhome_1\sqlplus\admin\glogin.sql
```



### 	设置列宽:

```
a 代表一个字符
9 代表一位数字
col ename for a10
col sal for 9999
```



### 命令行删除字符

ctrl +BACKSPACE键

![image-20220511192602044](/images/javawz/image-20220511192602044.png)



