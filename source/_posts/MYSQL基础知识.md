---
title: MYSQL基础知识
date: 2022-05-14 05:24:27
tags:
categories: 数据库
doc:
---

mysql被oracle公司收购了.

​	

```
mysql-essential-5.1.60-win32.msi 			精简版，如果只需要mysql服务，就选择此版本。

mysql-5.1.60-win32.msi 						是完整版，包含安装程序和配置向导，有MySQL文档。  

mysql-noinstall-5.1.60-win32.zip 			是非安装的zip压缩包，没有自动安装程序和配置向导，无安装向导

mysql-5.1.60.zip 							是用于windows的Mysql源码压缩包
```



###   mysql版本:

  		a Community社区版: 免费
  		b Enterprise企业版: 收费
  		1 MySQL标准版
  	    2 MySQL企业版 
  	    3 MySQL集群版
  	  每个版本又可以分为windows和linux版本, 再细分还可以分为32位和64位



### mysql服务器的安装和卸载

1 查询服务器上已经按照的mysql版本:

```
rpm -qa | grep mysql
```



2 卸载旧的版本

```
rpm -e 软件包名 --nodeps --allmatches (不理会依赖关系，删除所有上一步查出来的相同的mysql)
```



3 删除一些卸载时未能删除的文件

	#rm -f /etc/my.cnf
	#rm -rf /var/lib/mysql
	#rm -rf /var/share/mysql
	#rm -rf /usr/bin/mysql*


4 mysql的安装
解压 : 

```
unzip V46610-01-MySQL Database 5.6.20 RPM for Oracle Linux RHEL 6 x86 (64bit).zip
```

解压出来必须要安装的包: 
		

```
MySQL-client-advanced-5.6.20-1.el6.x86_64.rpm
MySQL-devel-advanced-5.6.20-1.el6.x86_64.rpm
MySQL-server-advanced-5.6.20-1.el6.x86_64.rpm			
```

按照命令: 

```
rpm -ivh MySQL-server-advanced-5.6.****-1.el6.x86_64.rpm
```

	安装完成之后密码的存放位置: /root/.mysql_secret


5 登陆mysql服务

a 用root身份启动mysql服务			

```
service mysql start
```

b 查看mysql服务是否已经启动

```
ps -u mysql
ps -ef | grep mysql
```



c  停止mysql服务

```
service mysql stop
```



d 登陆mysql服务

```
mysql -u 用户名 -pXXXX               XXXX代表密码

mysql -h 主机名 -P 端口号 -u 用户名 -p

mysql -h 主机名 -P 端口号 -u 用户名 -p密码
```



```
注意: 密码从/root/.mysql_secret文件中获取
	      
 登陆成功后, 修改密码:
	mysql> set password=password('123456'); //这个一般不使用了
	
	ALTER USER '用户名'@'localhost' IDENTIFIED BY '密码';     一般使用这个修改密码
    
    注意: 若无法正常登陆, 可以用root用户杀死mysql服务, 重新启动.
    	  
	如何将oracle用户加入到sudo当中:
		打开 /etc/sudoers, 添加oracle ALL=(ALL) ALL
```

5  退出登录

```
quit/exit
```



### 安装客户端

```
#rpm -ivh MySQL-client-advanced-5.6.****-1.el6.x86_64.rpm
说明:不安装mysql-client是不能使用mysql工具登陆到mysql数据库

其他软件包选择性安装：
#rpm -ivh MySQL-devel-advanced-5.6.20-1.el6.x86_64.rpm
```



### 远程访问不了数据库

在控制台输入 `mysql -uroot -p密码` 进入mysql

输入`use mysql; `进入mysql数据库 

输入` update user set host='%' where user='root' ;` 来允许通过ip访问。



修改 /etc/mysql/mysql.conf.d/下的mysqld.cnf文件

将`bind-address = 127.0.0.1`改成`bind-address =0.0.0.0`

```
 28 #
 29 # Instead of skip-networking the default is now to listen only on
 30 # localhost which is more compatible and is not less secure.
 31 bind-address        = 0.0.0.0
```

