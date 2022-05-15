---
title: mysql表的约束
date: 2022-05-15 03:33:15
tags:
categories: 数据库
doc:
---



```
*定义主键约束　primary key:	不允许为空，不允许重复
*定义主键自动增长　auto_increment
*定义唯一约束　unique
*定义非空约束　not null
*定义外键约束　constraint ordersid_FK foreign key(ordersid) references orders(id)
*删除主键：alter table tablename drop primary key ;
```



准备两个表:(下面的建表语句在执行的时候若不能正常执行, 可以尝试把tab去掉)

```
create table class (
id INT(11) primary key auto_increment,
name varchar(20) unique
);

create table student (
id INT(11) primary key auto_increment, 
name varchar(20) unique,
passwd varchar(15) not null,
classid INT(11),
constraint stu_classid_FK foreign key(classid) references class(id)
);
```



向class表中插入两条数据:

```
insert into class(name) values('音乐');
insert into class(name) values('体育');
insert into class(id, name) values(5, '美术');
insert into class(name) values('文化');

注意: 要插入的是部分列, 一定要在class表名后面写上列名, 表示要插入哪些列
	      由于class表的id是主键, 可以不用显示的插入主键的值, mysql会自动插入,而且会自动增长,确保不会重复.
```



向student表中插入数据:	

```
 正常插入一条记录:
	insert into student(name, passwd, classid) values('xiaohong', 'xxxxxx', 1);

1 测试主键的非空性
		insert into student(id, name, passwd, classid) values(null, 'xiaowen', 'xxxxxx', 1);
			注意: 若给主键插入一个null, mysql会自动插入一个有效的值, 所以mysql的主键肯定不会为空
			

2 测试主键的唯一性约束
		insert into student(id, name, passwd, classid) values(1, 'xiaoliu', 'xxxxxx', 1);
		----->ERROR 1062 (23000): Duplicate entry '1' for key 'PRIMARY'
	

3 测试name列的唯一性
		insert into student(name, passwd, classid) values('xiaohong', 'xxxxxx', 2);
		-----> ERROR 1062 (23000): Duplicate entry 'xiaohong' for key 'name'
		
		insert into student(name, passwd, classid) values(null, 'xxxxxx', 2);
		注意: name为unique约束, 只是不能重复, 但是可以为空
	

4 测试passwd的非空约束
		insert into student(name, passwd, classid) values('xiaohua', null, 2);
		-----> ERROR 1048 (23000): Column 'passwd' cannot be null
		

5 测试classid的外键约束
		insert into student(name, passwd, classid) values('meizi', 'mmmm', 10);
		----->OREIGN KEY (`classid`) REFERENCES `class` (`id`))
		在class表中没有id为10的列的值.
```





