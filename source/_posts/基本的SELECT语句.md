---
title: 基本的SELECT语句
date: 2022-05-11 19:47:23
tags:
categories: 数据库
doc:
---

SQL语句不区分大小写



### 基本的SELECT语句

​	   ` 其语法格式为：SELECT *|{[DISTINCT] column|expression [alias],...} FROM	table;`



### 案例: 

#### 1 查询所有员工的所有记录

```
select * from emp;
```

#### 2 查询员工号，姓名，月薪，奖金，年薪

```
select empno, ename, sal, comm, sal*12 from emp;
  
使用select * 和 select 列名的区别: 使用select *会先查有哪些列, 影响效率
```



#### 3 对案例2使用别名:

```
select empno, ename, sal as 工资, comm 奖金, sal*12 "年 薪" from emp;

关于别名的结论: 
    as可以省略
    如果别名中间有空格, 需要使用""引起来
```



#### 4 查询员工号，姓名，月薪，奖金，年薪，年收入

```
select empno, ename, sal, comm, sal*12 年薪, sal*12+nvl(comm, 0) 年收入 from emp;

结论: 
  	1 包含有null的表达式都为空
  	2 nvl的用法: nvl(a, b): 如果a为空, 则取b的值.
```

#### 5 查看员工表不同的部门编号

```
select distinct deptno from emp;
```



####  6 查看不同部门的不同工种

	select distinct detpno, job from emp; 
			  
	结论:
		distinct的作用范围??
		distinct作用于后面出现的所有的列.



#### 7 输出计算表达式 3+20*5，显示当前日期  sysdate

	select 3+20*5, sysdate from dual;
	
		注意: dual表是一个伪表, 主要是为了满足sql的语法规定
