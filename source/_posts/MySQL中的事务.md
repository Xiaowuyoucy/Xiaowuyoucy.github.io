---
title: MySQL中的事务
date: 2022-05-16 09:23:35
tags:
categories: 数据库
doc:
---

修改mysql为手工提交:

```
1 开启事务
	start transaction;
2 设置手工提交
	set autocommit=0;

1 开启事务
	start transaction;
2 设置自动提交
	set autocommit=1;
```





```c
//mysql中的事务
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "mysql.h"

#define SET_TRAN	"SET AUTOCOMMIT=0"  		//手动commit	————手动commit
#define UNSET_TRAN	"SET AUTOCOMMIT=1"			//自动commit

#define _HOST_ "127.0.0.1"
#define _USER_ "root"
#define _PASSWD_ "123456"
#define _DBNAME_ "scott"

//设置事务为手动提交
int mysql_OperationTran(MYSQL *mysql)  			
{
	//--开启事务
	int ret = mysql_query(mysql, "start transaction");  
	if (ret != 0) {
		printf("mysql_OperationTran query start err: %s\n", mysql_error(mysql));
		return ret;
	}

	//--设置事务为手动提交
	ret = mysql_query(mysql, SET_TRAN);			//set　autocommmit = 0
	if (ret != 0) {
		printf("mysql_OperationTran query set err: %s\n", mysql_error(mysql));
		return ret;
	}

	return ret;
}

//设置事务为自动提交
int mysql_AutoTran(MYSQL *mysql)
{
	//--开启事务
	int ret = mysql_query(mysql, "start transaction");  
	if (ret != 0) {
		printf("mysql_AutoTran query start err: %s\n", mysql_error(mysql));
		return ret;
	}

	//--设置事务为自动提交
	ret = mysql_query(mysql, UNSET_TRAN);  //"set autocommit = 1"
	if (ret != 0) {
		printf("mysql_AutoTran query set err: %s\n", mysql_error(mysql));
		return ret;
	}

	return ret;		
}

//执行commit，手动提交事务
int mysql_Commit(MYSQL *mysql)
{
	int ret = mysql_query(mysql, "COMMIT"); //提交
	if (ret != 0) {
		printf("commit err: %s\n", mysql_error(mysql));
		return ret;
	}
	return ret;
}

//执行rollback，回滚事务		
int mysql_Rollback(MYSQL *mysql)
{
	int ret = mysql_query(mysql, "ROLLBACK");
	if (ret != 0) {
		printf("rollback err: %s\n", mysql_error(mysql));
		return ret;
	}
	return ret;
	
}

#define DROP_SAMPLE_TABLE "DROP TABLE IF EXISTS test_table"
#define CREATE_SAMPLE_TABLE "CREATE TABLE test_table(col1 INT,\
                                                 col2 VARCHAR(10),\
                                                 col3 VARCHAR(10))"
                                                                                       
#define sql01 "INSERT INTO test_table(col1,col2,col3) VALUES(10, 'AAA', 'A1')"
#define sql02 "INSERT INTO test_table(col1,col2,col3) VALUES(20, 'BBB', 'B2')"
#define sql03 "INSERT INTO test_table(col1,col2,col3) VALUES(30, 'CCC', 'C3')"
#define sql04 "INSERT INTO test_table(col1,col2,col3) VALUES(40, 'DDD', 'D4')"

int main(void)
{
	int ret = 0;

	MYSQL *mysql = mysql_init(NULL);
	
	mysql = mysql_real_connect(mysql, _HOST_, _USER_, _PASSWD_, _DBNAME_, 0, NULL, 0);
	if (mysql == NULL) {
		ret = mysql_errno(mysql);
		printf("func mysql_real_connect() err：%d\n", ret);
		return ret;
	} 	
	printf(" --- connect ok......\n");	
	//执行删除表
	if (mysql_query(mysql, DROP_SAMPLE_TABLE)) {
	  fprintf(stderr, " DROP TABLE failed\n");
	  fprintf(stderr, " %s\n", mysql_error(mysql));
	  exit(0);
	}
	//执行创建表
	if (mysql_query(mysql, CREATE_SAMPLE_TABLE)) {
	  fprintf(stderr, " CREATE TABLE failed\n");
	  fprintf(stderr, " %s\n", mysql_error(mysql));
	  exit(0);
	}	
	
	ret = mysql_OperationTran(mysql); 	//开启事务，并修改事务属性为手动commit 
	if (ret != 0) {
		printf("mysql_OperationTran() err:%d\n", ret);
		return ret;
	}
	
	ret = mysql_query(mysql, sql01);	//向表中插入第一行数据 ‘AAA’
	if (ret != 0) {
		printf("mysql_query() err:%d\n", ret);
		return ret;
	}
	
	ret = mysql_query(mysql, sql02);	//向表中插入第二行数据 ‘BBB’
	if (ret != 0) {
		printf("mysql_query() err:%d\n", ret);
		return ret;
	}
	
	ret = mysql_Commit(mysql); 			//手动提交事务
	if (ret != 0) {
		printf("mysql_Commit() err:%d\n", ret);
		return ret;
	}
	//////////AAA BBB  进去了。
	
#if 1 
	ret = mysql_AutoTran(mysql); 		// =再次= 修改事务属性为【自动】commit
	if (ret != 0) {
		printf("mysql_OperationTran() err:%d\n", ret);
		return ret;
	}
#else 
	ret = mysql_OperationTran(mysql); 	// =再次= 修改事务属性为【手动】commit
	if (ret != 0) {
		printf("mysql_OperationTran() err:%d\n", ret);
		return ret;
	}
#endif

	ret = mysql_query(mysql, sql03);	//向表中插入第三行数据 ‘CCC’
	if (ret != 0) {
		printf("mysql_query() err:%d\n", ret);
		return ret;
	}
	
	ret = mysql_query(mysql, sql04);	//向表中插入第四行数据 ‘DDD’
	if (ret != 0) {
		printf("mysql_query() err:%d\n", ret);
		return ret;
	}
	
	ret = mysql_Rollback(mysql);		//直接rollback操作
	if (ret != 0) {
		printf("mysql_Rollback() err:%d\n", ret);
		return ret;
	}
	
	//rollback操作是否能回退掉CCC、DDD的值，取决于事务属性。
	
	mysql_close(mysql);
	
	return 0;	
}
```

