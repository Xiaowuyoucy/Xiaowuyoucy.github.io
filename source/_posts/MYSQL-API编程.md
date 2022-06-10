---
title: MYSQL_API编程
date: 2022-05-15 23:55:00
tags:
categories: 数据库
doc:
---

访问MySQL服务器，这需要使用mysqlclient库，MySQL的大多数客户端API（除Java和.NET）都是通过这个库来和MySQL服务器通讯的，而这个库正是使用C语言编写的。



### 查看当前系统内所使用的mysql数据库版本信息

```
mysql -V
```



### 需要用到的库

```
使用命令查找该库的路径
find / -name libmysqlclient*

命令可查看库内包含的函数。
nm /usr/lib64/mysql/libmysqlclient.a
```



### 头文件

```
用到头文件 <mysql.h> 可使用locate mysql.h查看其目录位置/usr/include/mysql/mysql.h。
```



### 编译引用了库的应用程序

```
gcc 源文件名.c -o 程序名 -I/usr/include/mysql/ -L/usr/lib64/mysql/ -lmysqlclient 
```



### MAKEFILE

```
src = $(wildcard *.c)
target = $(patsubst %.c, %, $(src))
inc_path = /usr/include/mysql/
lib_path = /usr/lib64/mysql/
all: $(target)
%:%.c
	gcc $< -o $@ -I$(inc_path) -L$(lib_path) -lmysqlclient -lstdc++ -lpthread -ldl -lrt
clean:
	-rm -rf $(target)
.PHONY: all clean

```



## MySQL API常用函数

```
使用MySQL库API函数的一般步骤：
	a. 初始化. 	MYSQL *mysql_init(MYSQL *mysql)
	
	b. 错误处理	unsigned int mysql_errno(MYSQL *mysql) 
	
					char *mysql_error(MYSQL *mysql);
					
	c. 建立连接.	
	MYSQL *mysql_real_connect(MYSQL *mysql, const char *host, const char *user, const char *passwd,const char *db, unsigned int port, const char *unix_socket, unsigned long client_flag);
	
	d. 执行SQL语句	int mysql_query(MYSQL *mysql, const char *stmt_str)
	
	e. 获取结果	MYSQL_RES *mysql_store_result(MYSQL *mysql)
	
				MYSQL_ROW mysql_fetch_row(MYSQL_RES *result)
				
	f. 释放内存	void mysql_free_result(MYSQL_RES *result)
	
	g. 关闭连接	void mysql_close(MYSQL *mysql)
```





### 初始化

编写程序测试 初始化函数

```
MYSQL *mysql_init(MYSQL *mysql);
```

其中有一种新数据类型MYSQL。可在头文件`mysql.h → 263. typedef struct st_mysql {...} MYSQL;`找到其定义。是一个结构体。

处理错误码的函数：

```
unsigned int mysql_errno(MYSQL *mysql) 
```



```
#include <stdio.h>
#include "mysql.h"

int main(void)
{
	int i, ret = 0, num = 0;

	MYSQL *mysql = mysql_init(NULL);
	if (mysql == NULL) {
	ret = mysql_errno(mysql);
	printf("mysql_init err:%d\n", ret);
	return ret;
	}
	printf("init ok...\n");

	return 0;
}
```





```
编译出错，原因是64位Linux环境下，动态库配置不完整。需手动指定编译所用的动态库。根据错误提示分析需要加入如下函数库：
1.	__gxx_personality_v0   		--> 	-lstdc++		使用g++相关的环境
2. dlclose/dlopen/dlsym		 		-->	-ldl			完成用一个程序加载其他动态库的作用。
3. pthread_*						-->  -lpthread		线程库
4. `my_getsystime'/`clock_gettime'	-->  -lrt			librt.so是glibc中对real-time的支持库

	使用ldd命令可以查看该可执行文件运行所依赖的库文件。
```





### 连接数据库关闭连接

mysql_connect();但该函数已经过时，应该使用手册中推荐的mysql_real_connect函数取而代之。

```
MYSQL *mysql_real_connect(MYSQL *mysql, const char *host, const char *user, const char *passwd, const char *db, unsigned int port, const char *unix_socket, unsigned long client_flag) ;
```

​	根据手册中的描述，我们可以使用基础的链接方式与MySQL数据库建立连接。

```
mysql = mysql_real_connect(mysql, "localhost", "root", "123456", "mydb61", 0, NULL, 0);
```

​	连接数据库成功。对表中数据进行访问，访问结束需调用void mysql_close(MYSQL *mysql) 函数关闭连接。该函数在断开连接的同时，还可以解除分配由mysql指向的连接句柄。

```
mysql_close(mysql);
```



### 读取数据

#### 执行SQL语句

mysql_query函数不单单能完成查询sql的功能，还能完成非select语句在c程序中的执行。是一个十分万能的c程序中执行SQL语句的函数。并且该函数本身直接支持静态SQL。查询以\0结尾的字符串。如果语句中包含二进制数据，则需要调用mysql_real_query来执行查询语句。

```
函数原型：int mysql_query(MYSQL *mysql, const char *query);	成功返回0，失败返回非0
```

```
char *psql = "select * from emp";
ret = mysql_query(mysql, psql);
```

若执行的是UPDATE, DELETE或INSERT语句，则可通过`mysql_affected_rows()`获知受影响的记录数。

若执行的是SELECT语句，查询结束后，查询结果被保存在mysql句柄中。需要使用获取结果集的API函数将结果集获取出来。有两种方式可以获取结果集。

##### 注意: mysql_query执行的SQL语句不应为语句添加终结分号（‘;’）或“\g”。



#### 获取结果集

一种方式是通过mysql_store_result()将整个结果集全部取回来。另一种方式则是调用mysql_use_result()初始化获取操作，但暂时不取回任何记录。视结果集的条目数选择获取结果集的函数。两种方法均通过mysql_fetch_row()来访问每一条记录。

​	`MYSQL_RES *mysql_store_result(MYSQL *mysql)` 成功返回`MYSQL_RES`结果集指针，失败返回NULL。

​	`MYSQL_RES`是一个结构体类型，可以从mysql.h头文件中找到该结构体的定义：

```
mysql.h → 308. typedef struct st_mysql_res {...} MYSQL_RES;
```

整体获取的结果集，保存在 MYSQL_RES 结构体指针中，通过检查mysql_store_result()是否返回NULL，可检测函数执行是否成功：

```
MYSQL_RES *result = mysql_store_result(mysql);
if (result == NULL) 
{
	ret = mysql_errno(mysql);
	printf("mysql_store_result error: %s\n", mysql_error(mysql));
	return ret;	
}
```

该函数调用成功，则SQL查询的结果被保存在result中，但我们不清楚有多少条数据。所以应使用游标的方式将结果集中的数据逐条取出。



#### 解析结果集

通过游标一行一行fetch结果集中的数据。根据游标使用的一般特性，应使用循环结构，到达结尾或者出错，返回NULL。

​	函数原型：`MYSQL_ROW mysql_fetch_row(MYSQL_RES *result) `成功返回下一行的MYSQL_ROW结构。如果没有更多要检索的行或出现了错误，返回NULL。-----MYSQL_ROW定义在118行

​	`select * from emp`  可以看到emp表一共有8列数据。可以循环将每行上每一列的数据显示到屏幕。

```
MYSQL_ROW row = NULL;				//typedef char **MYSQL_ROW;	
while ((row = mysql_fetch_row(result)))
{
	printf("%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\n", row[0],row[1],row[2],row[3],row[4],row[5],row[6],row[7]);
}
```

MYSQL_ROW的本质是` typedef char ** MYSQL_ROW;` 数据信息存储的形式如下图所示：

![image-20220516003930249](/images/javawz/image-20220516003930249.png)



```
从mysql.h头文件可查看MYSQL_ROW定义: 118. typedef char **MYSQL_ROW; /*return data as array of string*/
```



从上图分析MYSQL_ROW为什么被定义为char**类型呢？推测mysq_fetch_row()的函数实现大致思想如下：

```
char **mysql_fetch_row(){
	char **tmp = (char **) malloc(sizeof(char *) * 8);
	for (i = 0; i < 8; i++)
	{
		tmp[i] = (char *)malloc(50);
	}
	strcpy(tmp[0], "7369");
	strcpy(tmp[1], "SMITH");
	strcpy(tmp[2], "CLERK");
	...
	return tmp;
}
```



#### 释放结果集

​	结果集处理完成，应调用对应的函数释放所占用的内存。		

`void mysql_free_result(MYSQL_RES *result); `成功释放参数传递的结果集。没有失败情况。

```
mysql_free_result(result);
```



#### 获取列数

查看帮助手册可以看到，有两个函数具备获取列数的功能：

```
unsigned int mysql_field_count(MYSQL *mysql) 			从mysql句柄中获取有多少列。

unsigned int mysql_num_fields(MYSQL_RES *result) 		从返回的结果集中获取有多少列。
```

​	选择任意一种方式均可以完成该功能。

```
int num = mysql_field_count(connect); 
while (row = mysql_fetch_row(result)) 
{
for (i = 0; i < num; i++) {
	printf("%s\t", row[i]);
}
	printf("\n");
	//printf("%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\n", row[0],row[1],row[2],row[3],row[4],row[5],row[6],row[7]);
	}
```



#### 获取表头

获取表头的API函数同样有两个：

```
MYSQL_FIELD *mysql_fetch_fields(MYSQL_RES *result) 	全部获取

MYSQL_FIELD *mysql_fetch_field(MYSQL_RES *result) 	获取单个
```

​	MYSQL_FIELD也是一个结构体类型，其内部保存了选择列表项的信息，其中的name成员变量就保存着列名。可从头文件`mysql.h中94-116`行找到其定义。

```
MYSQL_FIELD *fields = NULL;
fields = mysql_fetch_fields(result);	//得到表头的结构体数组
for (i = 0; i < num; i++) {		//已通过 mysql_field_count	获取了总列数	
	printf("%s\t", fields[i].name);	//每一列的列名保存在name成员中 
}
printf("\n");
```



### 示例程序

```c
//mysql获取结果集
#include <stdio.h>
#include <stdlib.h>
#include "mysql.h"

int main()
{
	//初始化
	//MYSQL *mysql_init(MYSQL *mysql) 
	MYSQL *mysql = mysql_init(NULL);
	if(mysql==NULL)
	{
		printf("mysql init error\n");
		return -1;
	}
	printf("mysql init ok\n");

	//连接mysql数据库
	//MYSQL *mysql_real_connect(MYSQL *mysql, const char *host, const char *user, const char *passwd, const char *db, unsigned int port, const char *unix_socket, unsigned long client_flag) 
	MYSQL *conn = mysql_real_connect(mysql, "192.168.10.145", "root", "123456", "scott", 0, NULL, 0);
	if(conn==NULL)
	{
		printf("mysql_real_connect error,[%s]\n", mysql_error(mysql));
		return -1;
	}
	printf("connect mysql OK, [%p], [%p]\n", mysql, conn);

	//执行sql语句
	//int mysql_query(MYSQL *mysql, const char *query) 
	char sSQL[255] = "select * from mytest";
	int ret = mysql_query(conn, sSQL);
	if(ret!=0)
	{
		printf("mysql_query error, [%s]\n", mysql_error(mysql));
	}
	printf("mysql_query ok\n");

	//获取列数
	//unsigned int mysql_field_count(MYSQL *mysql) 	
	//unsigned int num = mysql_field_count(conn);

	//获取结果集
	//MYSQL_RES *mysql_store_result(MYSQL *mysql) 
	MYSQL_RES *results = mysql_store_result(conn);
	if(results==NULL)
	{
		printf("mysql_store_result error,[%s]\n", mysql_error(mysql));
		return -1;
	}
	printf("mysql_store_result ok\n");


	int i = 0;
	//获取列数
	//	unsigned int mysql_num_fields(MYSQL_RES *result) 
	unsigned int num = mysql_num_fields(results);

	//获取表头信息---列名
	//MYSQL_FIELD *mysql_fetch_fields(MYSQL_RES *result)
	MYSQL_FIELD *fields = mysql_fetch_fields(results);
	if(fields==NULL)
	{
		printf("mysql_fetch_fields error,[%s]\n", mysql_error(mysql));
		return -1;
	}
	//打印表头信息
	printf("+------+-----------+\n");
	for(i=0; i<num; i++)
	{
		printf("%s\t", fields[i].name);
	}
	printf("\n");
	printf("+------+-----------+\n");
	
	//获取结果集中每一行记录
	MYSQL_ROW row;
	while((row=mysql_fetch_row(results)))
	{
		for(i=0; i<num; i++)
		{
			printf("%s\t", row[i]);	
		}
		printf("\n");
	}
	printf("+------+-----------+\n");

	//释放结果集
	//void mysql_free_result(MYSQL_RES *result) 
	mysql_free_result(results);

	//关闭数据库连接
	mysql_close(conn);
	
	return 0;
}

```

