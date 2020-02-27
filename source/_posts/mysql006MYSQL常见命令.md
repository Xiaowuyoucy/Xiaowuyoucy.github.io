---
title: MYSQL常见命令
date: 2020-02-27 07:03:01
tags:
categories: MYSQL
doc:
---

# MYSQL常见命令

1.查看当前所有的数据库

`show databases;`

<br />

<br />



2.打开指定的库

`use 库名;`

<br />

<br />



3.查看当前库的所有表

`show tables;`

<br />

<br />



4.查看其他库的所有表

`show tables from 库名;`

<br />

<br />



5.创建表

```mysql
create table 表名(
	列名 列类型,
	列名 列类型,
	...
);
```

<br />

<br />



6.查看表结构

`desc 表名;`

<br />

<br />



7.查看数据库版本

方式一:登录到`mysql`服务端

`select version();`

<br />

方式二:没有登录到`mysql`服务端

`mysql --version`或者`mysql -V`

<br />

<br />

<br /><br />

<br />

<br />