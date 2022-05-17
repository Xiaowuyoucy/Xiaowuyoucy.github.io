---
title: ORACLE查看数据库的库名、实例名、域名、服务器以及服务名
date: 2022-05-15 10:00:15
tags:
categories: 数据库
doc:
---



```
一、oracle中：
1、查询数据库名：select name,dbid from v$database;或者show parameter db_name;
2、查询实例名：select instance_name from v$instance;或者show parameter instance_name;
3、查询数据库域名：select value from v$parameter where name='db_domain';或者show parameter domain;
4、查询数据库服务器：select value from v$parameter where name='service_name';或者show parameter service;或者show parameter names;
5、数据库服务名：此参数是数据库标识类参数，用service_name表示。
数据库如果有域，则数据库服务名就是全局数据库名；
如果没有，则数据库服务名就是数据库名。查询：show parameter service_name;

二、一般在PL/SQL下，Oracle 用户查询其他用户的表一定要使用user.table格式才能查询，每次用户名.表名嫌麻烦的话。可以使用同义词，意思将user.table访问格式改成table格式即可
创建同义词语句格式：
create public synonym table_name for user.table_name;
public很关键词，访问权限修饰，若想某表的授权用户也能用同义词查询，
必须要用public访问修饰符。
比如：
原本"表前缀.表名"格式才能访问select * from PAASDATA.paas_city_def;
create public synonym table_name for PAASDATA.paas_city_def;
创建同义词后直接用表名即可select * from paas_city_def;
————————————————
版权声明：本文为CSDN博主「我冷漠」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_43748615/article/details/117766206
```

