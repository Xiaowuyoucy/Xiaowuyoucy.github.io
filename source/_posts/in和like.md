---
title: in和like
date: 2022-05-11 21:16:07
tags:
categories: 数据库
doc:
---



## 3 在where条件中使用in--in后面是一个集合

### 1 查询部门号是10或者20的员工信息

```
select * from emp where deptno=10 or deptno=20;
select * from emp where deptno in(10,20);

思考: 可以在in的集合中使用null吗?
	select * from emp where deptno in(10,20,null);
	select * from emp where deptno=10 or deptno=20 or deptno=null;
```



### 2 查询不是10和20号部门的员工信息

```
select * from emp where deptno!=10 and deptno!=20;
select * from emp where deptno not in(10,20);
			  
思考: 若not in的集合中有null会怎么样呢?
	select * from emp where deptno not in(10,20,null);
	select * from emp where deptno!=10 and deptno!=20 and deptno!=null;
			  
	not in后面不能出现null
```



## 在where条件中使用like--模糊查找, 其中: '%'匹配任意多个字符。'_'匹配一个字符

#### 1 查询员工首字母是S的员工信息

```
 select * from emp where ename like 'S%';	 
```



#### 2 查询员工编号为79开头的员工信息

```
select * from emp where empno like '79%'; 
```



#### 3 查询名字为四个字母长度的员工信息

```
select * from emp where ename like '____'; 
```



####    4 查询员工姓名带_的员工信息

```
插入一条记录, 用于测试转义字符
insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (1000, 'TOM_CAT', 'CLERK', 9999, to_date('23-01-1982', 'dd-mm-yyyy'), 1200.00, null, 10);   
		 

select * from emp where ename like '%\_%' escape '\'; 
```



​	

