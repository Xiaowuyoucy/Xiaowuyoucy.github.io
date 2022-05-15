---
title: mysql中文乱码问题
date: 2022-05-15 04:01:30
tags:
categories: 数据库
doc:
---

### MYSQL客户端的字符集

下面的测试均在test数据库中进行的:

实验步骤
  使用两个linux客户端窗口, 一个用utf8登陆, 一个用gbk登陆

  mysql数据库默认情况下是使用的utf8登陆的: 





查看所有应用的字符集:

```
show variables like 'character%';
			
				执行结果:
			+--------------------------+----------------------------+
			| Variable_name            | Value                      |
			+--------------------------+----------------------------+
			| character_set_client     | utf8                       |
			| character_set_connection | utf8                       |
			| character_set_database   | utf8                       |
			| character_set_filesystem | binary                     |
			| character_set_results    | utf8                       |
			| character_set_server     | latin1                     |
			| character_set_system     | utf8                       |
			| character_sets_dir       | /usr/share/mysql/charsets/ |
			+--------------------------+----------------------------+		
```

通过画图解释mysql的字符集问题:	   

![image-20220515040327430](/images/javawz/image-20220515040327430.png)

客户端  ------  mysql服务端   ------ 数据库文件	

```
character_set_client
character_set_connection 
character_set_results  
客户端使用的字符集, 都为utf8
```



```
此时执行:
	在utf8字符集环境下插入数据:
	insert into mytest values(5, '小胡');
	select * from mytest;  ----正常显示
```



接下来开启另一个linux客户端, 使用gbk字符集进行登陆:

```
 mysql -uroot -p123456 --default_character_set=gbk 
 
 	切换到test用户下:
	查看所有应用的字符集:  
	show variables like 'character%';
	
		+--------------------------+----------------------------+
		| Variable_name            | Value                      |
		+--------------------------+----------------------------+
		| character_set_client     | gbk                        |
		| character_set_connection | gbk                        |
		| character_set_database   | utf8                       |
		| character_set_filesystem | binary                     |
		| character_set_results    | gbk                        |
		| character_set_server     | latin1                     |
		| character_set_system     | utf8                       |
		| character_sets_dir       | /usr/share/mysql/charsets/ |
		+--------------------------+----------------------------+ 
	
		此时客户端的字符集为: 
				character_set_client  
				character_set_connection 
				character_set_results  都为gbk.
			
			
			select * from mytest:  --不能正常显示

      在gbk环境下插入, 然后再到utf8环境下去查看同样也不能正常显示中文.
      
```

总结: 
  	1.   在utf8环境插入的中文不能用gbk查看;
     	2.   在gbk环境下插入的中文不能用utf8查看;
     	3.   在utf8环境下插入的中文要在utf8环境下查看;

4.  在gbk环境下插入的中文要在gbk环境下查看. 



### 2 操作系统的语言集



```
cat /etc/sysconfig/i18n
显示: LANG="zh_CN.UTF-8"	
操作系统的菜单按照zh_CN显示,  文件存储按照utf8
		
linux操作系统语言环境 和 用户的配置的语言环境LANG 相互影响
echo $LANG   ----->zh_CN.UTF-8
	  
修改用户下的.bash_profile 中的LANG，屏蔽操作系统的LANG设置或者export LANG=C临时设置也可以
然后在登陆到mysql数据库上:
登陆数据库:   
	mysql -uroot -p123456 
	use test
	show variables like 'character%'

				+--------------------------+----------------------------+
				| Variable_name            | Value                      |
				+--------------------------+----------------------------+
				| character_set_client     | latin1                     |
				| character_set_connection | latin1                     |
				| character_set_database   | utf8                       |
				| character_set_filesystem | binary                     |
				| character_set_results    | latin1                     |
				| character_set_server     | latin1                     |
				| character_set_system     | utf8                       |
				| character_sets_dir       | /usr/share/mysql/charsets/ |
				+--------------------------+----------------------------+
				
再次查看mytest表, 不能正常显示中文
		select * from mytest; 				
```

### 3 linux客户端的字符集

若将linux客户端设置问GB18030, 则也会对中文的显示产生影响: