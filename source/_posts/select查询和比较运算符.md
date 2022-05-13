---
title: select查询-比较运算符
date: 2022-05-11 20:38:00
tags:
categories: 数据库
doc:
---





### 	若在编写sql语句的时候, 书写错误, 则可以进行修改:

​	

```
如 SQL> select * ffom
	SQL> emp;
	先输入: 错误发生的那一行, 然后输入c /ffom/from,最后输入/执行sql语句.
	SQL> 2
	SQL> c/ffom/from
	SQL> /
```

​		

### 若编写的sql语句比较长, 可以使用edit打开一个记事本, 在记事本上编写:

![image-20220511204054544](/images/javawz/image-20220511204054544.png)

```
如: SQL>edit , 然后按/执行sql语句
语句末尾不需要加分号
最后以/结束
```

### 保存查询结果:

	spool名的使用: 
			spool d:\results
			select * from emp;
			spool off
		这样会将查询结果保存到文件中.


## where

```
基本结构: select .... from  table  where 条件;
```

### 使用比较运算符: > >= < <= != (<>) between and

#### 1 查询10号部门的员工信息

```
select * from emp where deptno=10;
```

#### 2 查询员工名字为king的员工信息

```
select * from emp where ename = 'KING';
  			     
结论: 表中的列的值是区分大小写的; 但是关键字不区分大小写
```

#### 3 查找薪水不等于1250员工的信息

```
select * from emp where sal!=1250;
select * from emp where sal<>1250;
```

#### 4 查询入职日期为1981年11月17日的员工信息

```
select * from emp where hiredate='1981-11-17'; --查询错误
select * from emp where hiredate='17-11月-81';
```

#### 查询当前使用的日期格式: 

```
select * from v$nls_parameters;
```

#### 修改日期格式:

```
alter session set NLS_DATE_FORMAT='yyyy-mm-dd';
```

#### 修改成原有的格式:

```
alter session set NLS_DATE_FORMAT='DD-MON-RR';	
说明: 需要注意日期格式, 默认是DD-MON-RR
```

#### 5 查找工资介于1000-2000之间的员工信息

```
select * from emp where sal>=1000 and sal<=2000;

select * from emp where sal between 1000 and 2000;
结论: between and是闭区间
```



  	

