---
title: where逻辑表达式
date: 2022-05-11 20:57:34
tags:
categories: 数据库
doc:
---

## 在where条件中使用逻辑运算符: or and not 	

#### 1 查询10号部门或者20部门的员工信息



```
select * from emp where deptno=10 or deptno=20;	
```



#### 2 查询10号部门员工工资为1300的员工信息

```
 select * from emp where deptno=10 and sal=1300;
```



#### 3 查询81年2月(含2月)至82年2月(不含2月)入职的员工信息(大于等于81年2月1日，小于等于82年1月31日)

```
 说明: 注意日期格式问题,注意月份单月不要在前面加0,否则会报错
			  select * from emp where hiredate>='1-2月-81' and hiredate<='31-1月-82';
			  select * from emp where hiredate between '1-2月-81' and '31-1月-82';
```



#### 结论: 关于and or 操作符的sql优化问题?

```
where条件在进行逻辑表达式计算的时候,是从右往左进行的, 所以对于and来说, 要把容易出现假的放在
最右边, 对于or来说, 要把容易出现真的表达式放在最右边.
where a and b and c and d;
where a or b or c or d;
```



#### 4 查询奖金为空的员工信息-null

```
select * from emp where comm=null; --不正确的写法

select * from emp where comm is null;
```



#### 5 查询奖金不为空的员工信息

```
select * from emp where comm!=null;
select * from emp where comm is not null;

结论: 关于null的在where条件中使用的结论?
		where条件后面为空应该用is null
		where条件后面不为空使用is not null
```



#### 分析下面的sql语句:

```
select * from emp where deptno=10 or deptno=30 and sal=1250;
		      
	**注意: 在有or和and的where条件中, and的优先级比or高, 所以若表示10部门或者20部门, 且sal为1250的, 应该
		   select * from emp where (deptno=10 or deptno=30) and sal=1250;
	 **结论: 在where条件表达式中有or的时候, 应该使用()括起来 
	
```

