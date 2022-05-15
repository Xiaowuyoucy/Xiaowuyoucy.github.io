---
title: mysql的基本操作
date: 2022-05-14 09:11:33
tags:
categories: 数据库
doc:
---



```
Query OK		//代表执行成功
```

```
库名和表名是区分大小写的
```

### 数据库CURD

对数据库进行增(create)、删(delete)、改(update)、查(Retrieve)操作。

知识点补充: 介绍mysql数据库与oracle数据库的不同(图解说明)
		mysql数据库: root用户 > 库 > 表
		oracle数据库: 数据库 > 用户 > 表

![image-20220514094433980](/images/javawz/image-20220514094433980.png)





```
“ ` ”（ESC键 下面的按键），表示反引号，默认情况下，反引号括起来的字符串，区分大小写。
```

1 创建数据库

```
创建数据库, 默认为latin1(拉丁)字符集的

create database 库名;

create database mydb1;
```

指定字符集为utf8

```
create database 库名 character set 字符集;
create database mydb2 character set utf8;
```

指定字符集为utf8,并对插入的数据进行检查是否是utf8字符集

```
create database 库名 character set 字符集 collate 字符集_general_ci;

create database mydb3 character set utf8 collate utf8_general_ci;
```

2 查看数据库

显示所有数据库

```
show databases;
```

显示创建数据库的语句信息

```
show create database 库名;
show create database mydb1;
```

注意 ：mysql默认语言集是latin1，每次在创建数据库的时候应指定字符集.



3 修改数据库

```
修改mydb1的字符集为utf8(不能修改数据库名)
alter database 库名 character set 字符集;

alter database mydb1 character set utf8;
```





4 删除数据库

	drop database 库名;
	
	drop database mydb1;




### 表的CURD

对表本身进行操作：创建，查看，修改，删除

```
在创建表之前要先指定使用哪个库, 先查看一下有哪些库:
查看有哪些库:
show databases;

使用指定的库:
use 库名;
use scott;
```



查看当前使用的是哪个库:

```
status  或者 select database() from dual;
```





![image-20220514094742022](/images/javawz/image-20220514094742022.png)



```
bit     		1位　	可以指定位数，如：bit(3)
int     		2字节 	可以指定最大位数，如：int<4>　最大为4位的整数
float   		2个字节　可以指定最大的位数和最大的小数位数，如：float<5,2> 最大为一个5位的数，小数位最多2位 
double　		   4个字节　可以指定最大的位数和最大的小数位数，如：float<6,4> 最大为一个6位的数，小数位最多4位
char　  		   必须指定字符数,如char(5) 为不可变字符　即使存储的内容为'ab',也是用5个字符的空间存储这个数据
varchar　	   必须指定字符数,如varchar(5) 为可变字符　如果存储的内容为'ab',占用2个字符的空间；如果为'abc',则占用3个字符的空间
text: 		    大文本(大字符串)
blob：		   二进制大数据　如图片，音频文件，视频文件
date: 			日期　如：'1921-01-02'
datetime: 		日期+时间　如：'1921-01-02 12:23:43'
timeStamp: 		时间戳，自动赋值为当前日期时间
```



```
创建表常用到的数据类型: 
常用的数据类型:
int
double
char 
varchar
timestamp
```



1 创建表

```
create table 表名(列名 类型, ..., 列名 类型, 列名 类型);

create table employee(empno int, ename varchar(20), sal int);
```

2 查看表

```
show tables;
```

查看表的创建语句

```
show create table 表名;
show create table emp;
```

查看表结构

```
desc 表名;
desc emp;
```



3 修改表

```
更改表名
rename table 旧表名 to 新表名;
rename table employee to worker;
	  	
	  
增加一个字段
alter table 表名 add column 列名 类型;
alter table worker add column email varchar(30);
	  	


修改一个字段名
    ALTER TABLE 表名 CHANGE [column] 旧字段名 新字段名 新数据类型;  
    ALTER TABLE worker CHANGE [column] email ema varchar(50);
     
修改一个字段类型
alter table 表名 modify column 列名 新类型;
alter table worker modify column email varchar(50);
	  	
	  
删除一个字段
alter table 表名 drop column 要删除的列名;
alter table worker drop column email;
	  	
	  	
修改表的字符集
alter table 表名 character set 字符集;
alter table worker character set utf8;
```



4 删除表

```
drop table 表名;
drop table worker;
注意: mysql删除表不能使用purge
```



### 表数据的CURD

1 创建一个表

```
create table 表名(
			列名 类型,
			列名 类型,
			列名 类型,
			列名 类型
		);
create table employee(
			id int,
			name varchar(20),
			sex int,
			birthday date,
			salary double,
			entry_date date,
			resume text
		);
	  注意: 若不能创建成功, 可以去掉建表语句中的tab键.
```

插入数据

```
insert into 表名 values(值1,值2,...,值n);		//插入全部列的数据

insert into employee values(1,'张三',1,'1983-04-27',15000,'2012-06-24','一个大牛');

//插入指定列的数据
insert into employee(列名1,列名2,列名3) values(值1,值2,值3);

insert into employee(id,name,sex,birthday,salary,entry_date,resume) values(2,'李四',1,'1984-02-22',10000,'2012-07-24','一个中牛');
insert into employee(id,name,sex,birthday,salary,entry_date,resume) values(3,'王五',0,'1985-08-28',7000,'2012-08-24','一个小虾');
```



2 查数据

```
select 列名1,...,列名n from 表名;

select id, name as "名字", salary "月薪", salary*12 年薪  from employee where id >=2;
		
select id, name as "名字", salary "月薪", salary*12 年薪  from employee where id >=2 \G
select id, name as "名字", salary "月薪", salary*12 年薪  from employee where id >=2 \g
		
说明: 若表的列太多, 可以使用\G或\g
```





3 update数据

```
update 表名 set 列名=值;

将所有员工薪水都增加500元。
update employee set salary=salary+500;

		
将王五的员工薪水修改为10000元，resume改为也是一个中牛
update employee set salary=10000, resume='也是一个中牛' where name='王五';
```



4 delete数据

```
delete from 表名;
delete from 表名 where 列名=值;
truncate table 表名;		//最好不要使用这一条sql语句,危险,删除后不可以恢复

删除表中姓名为王五的记录。
delete from employee where name='王五';

		
删除表中所有记录。
delete from employee;

		
使用truncate删除表中记录。
truncate table employee;
```

