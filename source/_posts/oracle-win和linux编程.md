---
title: oracle_win和linux编程
date: 2022-06-08 22:40:24
tags:
categories: 
doc:
---

### windows

```
#include <iostream>
#include <occi.h>
#include <string>
#include <stdlib.h>
#include <string.h>
using namespace std;
using namespace oracle::occi;

int main()
{
	//初始化环境
	Environment *ev = Environment::createEnvironment();
	if (ev == NULL)
	{
		cout << "createEnvironment failed" << endl;
		return -1;
	}

	//连接oracle数据库
	string user = "scott";
	string passwd = "tiger";
	string connstr = "192.168.10.145:1521/orcl";
	Connection *conn = ev->createConnection(user, passwd, connstr);
	if (conn == NULL)
	{
		cout << "connect database failed" << endl;
		return -1;
	}
	cout << "connect database OK" << endl;

	//sql语句中不要有分号
	/*string sql = "insert into dept values(109, 'lajiao', 'JYL_SC')";
	Statement *stmt = conn->createStatement(sql);
	stmt->execute();*/

	string sql = "delete from dept where deptno=99";
	Statement *stmt = conn->createStatement(sql);
	stmt->executeUpdate();

	//关闭连接
	ev->terminateConnection(conn);

	//释放环境资源
	Environment::terminateEnvironment(ev);
	return 0;
}

```



### Linux



```
#include <iostream>
#include <occi.h>
#include <string>
#include <stdlib.h>
#include <string.h>
using namespace std;
using namespace oracle::occi;

int main()
{
	//初始化环境
	Environment *env = Environment::createEnvironment();
	if(env==NULL)
	{
		cout << "create Environment error" << endl;
		return -1;
	}
	
	//Connection *Environment::createConnection( 
    //const string &userName,const string &password, const string &connectString );
	string user="scott";
	string passwd="tiger";
	string connstr="192.168.10.145:1521/orcl";
	Connection *conn = env->createConnection(user, passwd, connstr);
	if(conn==NULL)
	{
		cout <<"connect database error" << endl;
		return -1;
	}
	cout << "connect database OK!" << endl;
	
	Statement *stmt = NULL;
	string sql = "insert into dept values(11, 'training', 'changping')";
	//创建Statement对象
	//第一种方法
	/*stmt = conn->createStatement(sql);
	stmt->execute();*/
	
	//第二种方法
	/*sql = "insert into dept values(21, 'sport', 'JYL')";
	stmt = conn->createStatement();
	stmt->setSQL(sql);
	stmt->execute();*/
	
	//第三种方法
	/*sql = "insert into dept values(31, 'SALES', 'JYL')";
	stmt = conn->createStatement();
	stmt->execute(sql);*/
	
	//第四种方法
	/*sql = "insert into dept values(41, 'XIANGLIAO', 'JYL')";
	stmt = conn->createStatement();
	int num = stmt->executeUpdate(sql);
	cout << "num==" << num << endl;*/
	
	//第五种方法
	/*sql = "insert into dept values(51, 'huoguo', 'JYL')";
	stmt = conn->createStatement(sql);
	int num = stmt->executeUpdate();
	cout << "num==" << num << endl;*/
	
	//第六种方法
	/*sql = "insert into dept values(:1, :2, :3)";
	stmt = conn->createStatement(sql);
	stmt->setInt(1, 61);
	stmt->setString(2, "huajiao");
	stmt->setString(3, "JYL_SC");
	stmt->executeUpdate();*/
	
	//执行delete操作
	/*sql = "delete from dept where deptno=11";
	stmt = conn->createStatement(sql);
	stmt->executeUpdate();*/
	
	//使用占位符的删除操作
	/*sql = "delete from dept where deptno=:1";
	stmt = conn->createStatement(sql);
	stmt->setInt(1, 21);
	stmt->executeUpdate();*/	
	
	//执行update操作
	/*sql = "update dept set dname='xxxxx' where deptno=31";
	stmt = conn->createStatement(sql);
	stmt->executeUpdate();*/
	
	//使用占位符的update操作
	/*sql = "update dept set dname='yyyy' where deptno=:1";
	stmt = conn->createStatement(sql);
	stmt->setInt(1, 41);
	stmt->executeUpdate();*/
	
	//执行select查询
	/*sql = "select * from dept";
	stmt = conn->createStatement(sql);*/
	
	sql = "select * from dept where deptno>:1";
	stmt = conn->createStatement(sql);
	stmt->setInt(1, 30);
	ResultSet *rs = stmt->executeQuery();
	
	//循环获取结果集的每一行记录的值
	while(rs->next())
	{
		cout << rs->getInt(1) << " " << rs->getString(2) << " " << rs->getString(3) << endl;
	}
	//关闭结果集
	stmt->closeResultSet(rs);
	
	
	//端口数据库连接
	env->terminateConnection(conn);
	
	//关闭环境
	Environment::terminateEnvironment(env);
}

```

