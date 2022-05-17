---
title: mysql客户端开发
date: 2022-05-16 07:55:50
tags:
categories: 数据库
doc:
---

### 思路分析

```
mysql客户端编写思路分析:
1 mysql初始化--mysql_init
2 连接mysql数据库---mysql_real_connect
3 while(1)
  {
  	//打印提示符:write(STDOUT_FILENO, "mysql >", strlen("mysql >"));
  	//读取用户输入: read(STDIN_FILENO, buf, sizeof(buf))
  	//判断用户输入的是否为退出: QUIT quit exit EXIT
  	if(strncasecmp(buf, "exit", 4)==0 || strncasecmp(buf, "quit", 4)==0)
  	{
  		//关闭连接---mysql_close();
  		exit();
  	}
  	
  	//执行sql语句--mysql_query();
  	
  	//若不是select查询, 打印执行sql语句影响的行数--mysql_affected_rows();
  	if(strncasecmp(buf, "select", 6)!=0)
  	{
  		printf("Query OK, %d row affected", mysql_affected_rows());
  		continue;
  	}
  	
  	//若是select查询的情况
  		---//获取列数: mysql_field_count()
  	//获取结果集: mysql_store_result()
  		--获取列数: int mysql_num_fields();
  	//获取表头信息并打印表头信息:mysql_fetch_fields();
  		
  	//循环获取每一行记录并打印: mysql_fetch_row()
  	//释放结果集: mysql_free_result()
  	
  }
  
4 关闭连接: mysql_close();
```

1. 仿照mysql工具，应在连接数据库成功之后，在一个while循环中不断的接受用户输入的SQL语句。定义char sqlbuf[1024] 存储用户输入的SQL语句。初始化该buf，并提示用户输入SQL语句。使用gets函数在循环中动态接收用户输入。

​		

```
while (1) {

	memset(sqlbuf, 0, sizeof(sqlbuf));

	printf("\nYourSQL> ");

	fgets(sqlbuf, sizeof(sqlbuf), stdin);

}
```

2. 在mysql_query(connect, sqlbuf)之前，如果用户输入了“exit”那么程序直接结束。

3. 在执行完 mysql_query(connect, sqlbuf)之后，应该判别用户输入的是否为select语句。如不是select语句不需要查询结果集、处理结果集等繁复操作。

4. 如用户输入的是有结果集的SQL语句，将获取列数、获取结果集、获取表头、解析结果集、释放结果集等相关代码一起并入if (strncmp(sqlbuf, "select", 6))中。

​	测试注意：执行SQL语句时不要在结尾加“;”	





### 中文问题：

​	修改`mysql_real_connect()`参数，连接到表中有中文数据的数据库，如mydb2，执行程序，测试显示中文出现乱码。我们可以使用mysql_query函数来解决该问题。  

​	在 while (1) 之前使用` ret = mysql_query(mysql, "set names utf8"); `来设置查询属性(也可以加到while中)。表示在查询的时候使用utf8的形式进行查询。

或者`mysql_set_character_set(mysql, "utf8");`

获取当前使用的字符集:  `const char *mysql_character_set_name(MYSQL *mysql)`



### 程序实现

```c
//模拟mysql客户端程序
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include "mysql.h"

int main()
{
	//mysql初始化---mysql_init
	MYSQL *mysql = mysql_init(NULL);
	if(mysql==NULL)
	{
		printf("mysql_init error\n");
		return -1;
	}
	
	//连接数据库
	MYSQL *conn = mysql_real_connect(mysql, "localhost", "root", "123456", "scott", 0, NULL, 0);
	if(conn==NULL)
	{
		printf("connect mysql db error, [%s]\n", mysql_error(mysql));
		return -1;
	}
	
	//获取当前进程使用的字符集
	printf("before:[%s]\n", mysql_character_set_name(conn));
	
	//设置字符集为utf8格式
	mysql_set_character_set(conn, "utf8");
	printf("after:[%s]\n", mysql_character_set_name(conn));

	int i;
	int n;
	int ret;
	int num;
	char *p;
	char buf[1024];
	MYSQL_RES *results;
	MYSQL_FIELD *fields;
	MYSQL_ROW row;
	
	//进入循环等待用户输入sql语句并执行sql语句
	while(1)
	{
		//打印提示符
		write(STDOUT_FILENO, "mysql> ", strlen("mysql> "));
		
		//读取用户输入
		memset(buf, 0x00, sizeof(buf));
		read(STDIN_FILENO, buf, sizeof(buf));
		
		//1-去掉末尾的;
		p = strrchr(buf, ';');
		if(p!=NULL)
		{
			*p = '\0';
		}
		
		//2-去掉回车
		if(buf[0]=='\n') 
		{
			continue;
		}
		
		//去掉最前面的几个空格
		for(i=0; i<strlen(buf); i++)
		{
			if(buf[i]!=' ')
			{
				break;
			}
		}
		n = strlen(buf);
		memmove(buf, buf+i, n-i+1); //+1表示多拷贝一个\0
		printf("[%s]\n", buf);
		
		//若输入的是退出: exit EXIT quit QUIT
		if(strncasecmp(buf, "exit", 4)==0 || strncasecmp(buf, "quit", 4)==0)
		{
			mysql_close(conn);
			exit(0);
		}
		
		//执行sql语句
		ret = mysql_query(conn, buf);
		if(ret!=0)
		{
			printf("%s\n", mysql_error(conn));
			continue;
		}
		
		//若用户输入的不是select查询
		if(strncasecmp(buf, "select", 6)!=0)
		{
			printf("Query OK, %ld row affected\n", mysql_affected_rows(conn));
			continue;
		}
		
		//下面是select查询的情况	
		//获取结果集
		results = mysql_store_result(conn);
		if(results==NULL)
		{
			printf("%s\n", mysql_error(conn));
			continue;
		}
		
		//获取列数
		num = mysql_num_fields(results);
		
		//获取表头---列名
		fields = mysql_fetch_fields(results);
		if(fields==NULL)
		{
			printf("%s\n", mysql_error(conn));
			mysql_free_result(results);
			continue;
		}
		//打印表头
		printf("+----------+-----------+\n");
		for(i=0; i<num; i++)
		{
			printf("%s\t", fields[i].name);
		}
		printf("\n");
		printf("+----------+-----------+\n");
		
		//循环获取每一行记录
		while(row=mysql_fetch_row(results))
		{
			for(i=0; i<num; i++)
			{
				printf("%s\t", row[i]);
			}
			printf("\n");
		}
		printf("+----------+-----------+\n");
		
		//释放结果集
		mysql_free_result(results);
	}
		
	//关闭数据库连接
	mysql_close(conn);
	
	return 0;
}

```

