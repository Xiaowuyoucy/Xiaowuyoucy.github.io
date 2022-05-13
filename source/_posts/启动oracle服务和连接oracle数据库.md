---
title: 启动oracle服务和连接oracle数据库
date: 2022-05-09 21:31:35
tags:
categories: 数据库
doc:
---

### 	windows下的oracle的启动:

​		启动OracleServiceORCL
​		启动home1TNSListener
​		

## 在本机登陆：

- 普通用户身份登陆
  - sqlplus  ↙ 用户名 ↙	密码 ↙
  - sqlplus 用户名/密码，如sqlplus scott/tiger

- 以管理员身份登陆
  - sqlplus  /  as  sysdba（此处不用输入密码，在安装的时候已经输入密码）
  - sqlplus sys/sys as sysdba

### linux下启动oracle数据库:

#### 	使用linux的oracle用户登陆:

```
sqlplus sys/sys as sysdba
sqlplus / as sysdba	


SQL> startup   ---启动数据库服务
SQL> shutdown immeidate   ----关闭数据库服务			
```

​       	   

#### 启动监听服务:

```
lsnrctl start
```

#### linux下停止oracle数据库:

##### 停止监听服务:

```
lsnrctl stop
```



## 通过网络使用客户端远程登陆

远程通过网络登陆数据库需要安装oracle客户端软件，并进行配置才能使用，可通过使用net manager进行配置，配置完成之后可以使用连接字符串进行登陆，连接字符串中包含了数据库服务的IP地址和端口，以及实例名。

注意：安装oracle客户端的时候，安装路径中不能出现中文和空格，安装的时候选择管理员模式。

D:\oracle\app\HGUANG\product\11.2.0\client_1\network\admin\tnsnames.ora，下面是经过Net Manager进行配置后得到的一个文件内容：

![img](/images/javawz/wpsDA00.tmp.jpg) 

客户端安装完成之后进行远程登陆之前最好先进行测试：

首先测试网络是否是通的： `ping  IP `， 然后`tnsping  IP ` 或者 `tnsping oracle_orcl`。

### 使用sqlplus登陆oracle数据库:

```
普通用户登陆:  sqlplus scott/tiger@192.168.10.145/orcl
			 sqlplus scott/tiger@orcl/orcl

sys用户登陆:  sqlplus sys/sys as sysdba
```

​	

### 解锁用户:

```
alter user scott account unlock;
```

### 加锁用户:

```
alter user scott account lock;

需要用sys用户来加锁或者解锁,加锁之后用户就登录不了了
```



### 修改用户密码: 

```
password 用户名
或
alter user 用户名 identified by xxxxx;
```

### 查看当前的语言环境: 

```
select userenv('language') from dual;
```

​	

### 查看当前用户

```
show user
```



### 切换账号

```
conn 账号/密码@服务名 as sysdba
或
conn 账号/密码@服务名 
```



### 配置net manager

以管理员身份运行net manager

![image-20220511173750444](/images/javawz/image-20220511173750444.png)





![image-20220511173541273](/images/javawz/image-20220511173541273.png)



![image-20220511173602566](/images/javawz/image-20220511173602566.png)







![image-20220511173623244](/images/javawz/image-20220511173623244.png)





![image-20220511173646983](/images/javawz/image-20220511173646983.png)





![image-20220511173701089](/images/javawz/image-20220511173701089.png)



### PLSQL Developer客户端登录

以管理员身份运行PLSQL Developer

![image-20220511183835838](/images/javawz/image-20220511183835838.png)



可以选择命令窗口或sql窗口执行语句

![image-20220511183942335](/images/javawz/image-20220511183942335.png)



![image-20220511184026372](/images/javawz/image-20220511184026372.png)