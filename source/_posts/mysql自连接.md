---
title: mysql自连接
date: 2022-05-15 03:23:37
tags:
categories: 数据库
doc:
---

自连接:

查询员工、老板信息，显示: xxx的老板是yyy 

分析: 可以将emp表当成两个表来用, 一个是员工表 e, 一个是老板表 b



1 mysql不支持用||连接两个字符串



2 使用concat函数

```
select concat(e.ename, '的老板是', b.ename) from emp e, emp b where e.mgr=b.empno;
select concat(e.ename, '的老板是', b.ename) from emp e inner join emp b on e.mgr=b.empno;
```



3 若要显示KING的信息, 需要使用外连接

```
a mysql不支持nvl函数

b 使用ifnull函数
左外连接:
select concat(e.ename, '的老板是', b.ename) from emp e left outer join emp b on e.mgr=b.empno;
select concat(e.ename, '的老板是', ifnull(b.ename, 'HIMSELF')) from emp e left outer join emp b on e.mgr=b.empno;
右外连接:
select concat(e.ename, '的老板是', ifnull(b.ename, 'HIMSELF')) from emp b right outer join emp e on e.mgr=b.empno;
```



​		

