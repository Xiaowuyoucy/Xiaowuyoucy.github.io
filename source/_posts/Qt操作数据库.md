---
title: Qt操作数据库
date: 2022-06-12 23:53:03
tags:
categories: Qt
doc:
---

### Qt 编译oracle的OCI驱动

源码安装的时候必须选择Sources选项, 里边有数据库驱动的源码

![image-20220614222417323](/images/javawz/image-20220614222417323.png)

1. 版本和路径说明:
   -  Qt版本是Qt5.9.0，安装路径是默认的 C:\Qt\Qt5.9.0
   - Oracle客户端版本oracle 11g 安装路径是C:\Oracle\product\11.2.0\client_1
   - 打开C:\Qt\Qt5.9.0\5.9\Src\qtbase\src\plugins\sqldrivers\oci\ 目录下面的oci.pro。
2. 使用QtCreator打开oci.pro
   - 未进行修改的直接编译结果： Library 'oci' is not defined.
   - 修改oci.pro文件
   - ![image-20220614222805400](/images/javawz/image-20220614222805400.png)
3. 之后再重新编译，就会发现编译通过了，这时候我们在Qt安装的 根目录下面去找 “:\plugins”会发现里面有一个“ sqldrivers ” 文件夹，将其复制到Qt的c:\Qt\Qt5.9.0\5.9\mingw53_32\plugins下 面即可。
   - 如果Qt安装根目录在C盘, 那么plugins就在C盘根目录
   - 编译的时候建议编译两个版本
     - debug
     -  release



以上方法对Qt5.8以后的版本都适用.



## QT操作mysql数据库

Header: include 

qmake: QT += sql 

首先需要在QT项目文件的*.pro文件中添加: QT += core gui sql 

所用到的头文件:

```
#include <qDebug>
#include <QSqlDatabase>
#include <QSqlQuery>
#include <QSqlRecord>
#include <QSqlError>
```

### 1.加载数据库驱动

**将libmysql.dll库放到下面的目录下: D:\QT\Qt5.10.1\5.10.1\mingw53_32\bin**

```cpp
//查看QT支持的驱动
qDebug() << QSqlDatabase::drivers();
输出结果为:
//("QSQLITE", "QMYSQL", "QMYSQL3", "QOCI", "QOCI8", "QODBC", "QODBC3", "QPSQL", "QPSQL7")
QSqlDatabase db = QSqlDatabase::addDatabase("QMYSQL");
```

### 2.设置账号密码

```CPP
设置账号和密码信息使用QSqlDatabase类的成员函数:
void setHostName(const QString &host)
void setPassword(const QString &password)
void setPort(int port)
void setUserName(const QString &name)
void setDatabaseName(const QString &name)
使用方法, 如下所示
db.setHostName("192.168.10.145"); //设置mysql主机的IP地址
db.setDatabaseName("scott"); //设置数据库名
db.setUserName("root"); //设置用户名
db.setPassword("123456"); //设置密码
其实上面这几步就类似于登录mysql数据库需要的关键信息:
mysql -h192.168.10.145 -uroot -p123456 scott

```

### 3.打开数据库

```cpp
bool QSqlDatabase::open()
详情查看QT帮助手册
使用方法, 如下所示:
if(!db.open())
{
    qDebug() << "数据库操作失败";
    return;
}

```

### 4.执行select查询操作

```cpp
1 需要使用QSqlQuery类的相关函数:
Header: #include <QSqlQuery>

2 QSqlQuery类的构造函数:
QSqlQuery(const QString &query = QString(), QSqlDatabase db = QSqlDatabase())
该构造函数都有默认值, 构造的时候可以不指定:
用法: QSqlQuery query;

3 执行select查询语句
	3.1 第一种方法, 直接调用exec执行sql语句
		例如: query.exec("select * from dept");
	3.2 第二种方法,先调用prepare准备一个sql语句, 然后再执行exec执行sql语句
		例如:
            bool success;
            query.prepare("select * from dept");
            success = query.exec();
            if(!success)
            {
            qDebug() << "查询失败";
            return;
            }


4 获取查询表的总字段数
	先调用QSqlQuery类的record方法:
		QSqlRecord record() const;
	然后在调用QSqlRecord类的count方法
		int QSqlRecord::count() const
	例如:
        QSqlRecord rec = query.record();
        qDebug() << "查询结果字段总数为：" << rec.count();
        //注意: 有的函数有代码示例, 可以直接参考


5 获取查询结果总记录数
	调用QSqlQuery类的size方法:
		int size() const
	例如:
		qDebug() << "查询结果记录总数为" << query.size();


6 遍历查询的结果集
	获取每一条记录:
		bool QSqlQuery::next()
	通过列的索引位置获取列的值---列的索引从0开始
		QVariant QSqlQuery::value(int index) const
	通过列名获取列的值:
		QVariant value(const QString &name) const
	代码示例:
        while(query.next())
        {
        	//qDebug() << query.value(0).toInt() << query.value(1).toString() << query.value(2).toString();
        	qDebug() << query.value("deptno").toInt() << query.value("dname").toString() <<
       		 query.value("loc").toString();
        }


7 移动指向结果集的位置指针:
	bool QSqlQuery::seek(int index, bool relative = false)
	例如:
		query.seek(-1); //每次next都会使记录指针移动一次, 可以使用seek函数重置指针位置,类似于文件指针
```

### 5.执行insert操作

```cpp
方法1:
    query.prepare("insert into dept values(77, 'sports', 'xiuzheng')");
    success = query.exec();
    if(!success)
    {
    QSqlError lastError = query.lastError();
    qDebug() << "插入失败：" << lastError.driverText() << lastError.databaseText();
    return;
    }
方法2: 使用带有占位符的sql语句, 该语句不是一个完整的sql语句,需要调用bindValue函数给占位符设置值.
    query.prepare("insert into dept values(?, ?, ?)");
    //给字段设置值,字段位置索引从0开始
    query.bindValue(0, 99);
    query.bindValue(1, "SPORTS");
    query.bindValue(2, "BEIJING");
    success = query.exec();
    if(!success)
    {
    QSqlError lastError = query.lastError();
    qDebug() << "插入失败：" << lastError.driverText() << lastError.databaseText();
    return;
    }
方法3:直接调用exec并将sql语句作为参数也可以直接插入
    success = query.exec("insert into dept values(66, 'SALES', 'SHANGHAI')");
    if(!success)
    {
    QSqlError lastError = query.lastError();
    qDebug() << "插入失败：" << lastError.driverText() << lastError.databaseText();
    return;
    }
```

### 6.执行update操作

```cpp
方法1:直接调用execl并将sql语句作为参数执行
    success = query.exec("update dept set loc='MEIGUO' where deptno=99");
    if(!success)
    {
    QSqlError lastError = query.lastError();
    qDebug() << "update failed" << lastError.driverText() << lastError.databaseText();
    return;
    }
方法2:使用带有占位符占位符的sql语句
    query.prepare("update dept set loc=? where deptno=?");
    query.bindValue(0, "JAPAN");
    query.bindValue(1, 77);
    success = query.exec();
    if(!success)
    {
    QSqlError lastError = query.lastError();
    qDebug() << "update failed" << lastError.driverText() << lastError.databaseText();
    return;
    }

```

### 7.执行delete操作

```CPP
方法1:直接调用execl并将sql语句作为参数执行
	query.exec("delete from dept where deptno=99");
方法2:使用带有占位符的sql语句
    query.prepare("delete from dept where deptno=? or loc=?");
    query.bindValue(0, 77);
    query.bindValue(1, "SHANGHAI");
    success = query.exec();
    if(!success)
    {
    QSqlError lastError = query.lastError();
    qDebug() << "update failed" << lastError.driverText() << lastError.databaseText();
    return;
    }

```

### 8.事务处理

```CPP
1 开启事务
	query.exec("START TRANSACTION");
	
2 设置自动提交和手动提交---->默认情况下mysql是自动提交的
	query.exec("SET AUTOCOMMIT=0"); //手动提交
	query.exec("SET AUTOCOMMIT=1"); //自动提交
	
3 事务的提交和回滚操作
    query.exec("COMMIT");
    query.exec("ROLLBACK");
测试方法: 先开启一个新的事务, 并设置为手动提交, 然后插入数据, 最后回滚, 看数据是否已经插入到数据库中;然
后在修改为提交, 查看数据是否已经插入到数据库中.
测试代码如下:
    query.exec("START TRANSACTION");
    query.exec("SET AUTOCOMMIT=0"); //手动提交
    success = query.exec("insert into dept values(99, 'SALES', 'SHANGHAI')");
    if(!success)
    {
        QSqlError lastError = query.lastError();
        qDebug() << "update failed" << lastError.driverText() << lastError.databaseText();
        //回滚事务
        query.exec("ROLLBACK");
        return;
    }
    //提交事务
    query.exec("COMMIT");

```

### 9.关闭数据库

```CPP
//关闭数据库
db.close();
```

参考网站: https://blog.csdn.net/wyansai/



## 创建sqlite数据库的过程---同mysql

![image-20220615120258914](/images/javawz/image-20220615120258914.png)

![image-20220615120310264](/images/javawz/image-20220615120310264.png)

![image-20220615120320100](/images/javawz/image-20220615120320100.png)

建好sqlite数据库之后, 可以看到如下图所示:

![image-20220615120339740](/images/javawz/image-20220615120339740.png)

![image-20220615120350206](/images/javawz/image-20220615120350206.png)

参考网站: https://www.cnblogs.com/xia-weiwen/archive/2017/05/04/6806709.html 

参考代码:

```
在QT上建立一个控制台应用程序, 然后将代码直接复制上就可以编译测试.
还有就是需要在*.pro文件中添加: QT += sql
#include <QCoreApplication>
#include <QSqlDatabase>
#include <QSqlError>
#include <QSqlQuery>
#include <qDebug>
#include <QString>
int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);
    bool success;
    //建立数据库--加载数据库驱动
    QSqlDatabase database;
    if (QSqlDatabase::contains("qt_sql_default_connection"))
    {
   		 database = QSqlDatabase::database("qt_sql_default_connection");
    }
    else
    {
        database = QSqlDatabase::addDatabase("QSQLITE");
        database.setDatabaseName("C:\\Users\\Administrator\\Desktop\\sqlite.db");
        //db.setDatabaseName("main");//有这个会报错
        //database.setUserName("");//可以省略
        //database.setPassword("");//可以省略
    }
    //打开数据库
    
    if (!database.open())
    {
        qDebug() << "Error: Failed to connect database." << database.lastError();
        return -1;
    }
    //#####################################################################
    //--新建一个表student作为测试表
    //#####################################################################
    //执行sql语句---新建一个表
    QSqlQuery sql_query;
    /*QString create_sql = "create table student (id int, name varchar(30), age int)";
    sql_query.prepare(create_sql);
    success = sql_query.exec();
    if(!success)
    {
        qDebug() << "Error: Fail to create table." << sql_query.lastError();
        return -1;
    }
    else
    {
   		 qDebug() << "Table created!";
    }*/
    //#####################################################################
    //--插入数据操作
    //#####################################################################
    //向student表中插入数据
    //第一种方法:
    /*success = sql_query.exec("insert into student values(1, 'xiaowen', 10)");
    if(!success)
    {
        qDebug() << "Error: Fail to create table." << sql_query.lastError();
        return -1;
    }*/
    //第二种方法:
    QString sql = "insert into student values(3, 'xiaozi', 25)";
    success = sql_query.exec(sql);
    if(!success)
    {
        qDebug() << "Error: Fail to create table." << sql_query.lastError();
        return -1;
    }
    //第三种方法:执行带有占位符的sql语句
    QString insert_sql = "insert into student values (?, ?, ?)";
    sql_query.prepare(insert_sql);
    /*sql_query.addBindValue(2);
    sql_query.addBindValue("xiaohu");
    sql_query.addBindValue(22);*/
    sql_query.bindValue(0, 5);
    sql_query.bindValue(1, "小马");
	sql_query.bindValue(2, 30);
    success = sql_query.exec();
    if(!success)
    {
        qDebug() << sql_query.lastError();
        return -1;
    }
    else
    {
   		 qDebug() << "inserted success!";
    }
    //批量插入
    insert_sql = "insert into student values(?,?,?)";
    sql_query.prepare(insert_sql);
    QVariantList GroupIDs;
    GroupIDs.append(0);
    GroupIDs.append(1);
    GroupIDs.append(2);
    QVariantList GroupNames;
    GroupNames.append("xiaoma");
    GroupNames.append("xiaowei");
    GroupNames.append("xiaolian");
    QVariantList GroupAddress;
    GroupAddress.append(10);
    GroupAddress.append(15);
    GroupAddress.append(18);
    sql_query.addBindValue(GroupIDs);
    sql_query.addBindValue(GroupNames);
    sql_query.addBindValue(GroupAddress);
    success = sql_query.execBatch();
    if(!success)
    {
        qDebug()<<sql_query.lastError();
        return -1;
    }
    else
    {
        qDebug()<<"insert batch data succ";
    }
    return 0;
    //#####################################################################
    //--更新数据
    //#####################################################################
    //QString update_sql = "update student set name = :name where id = :id";
    QString update_sql = "update student set name = ? where id = ?";
    sql_query.prepare(update_sql);
    //sql_query.bindValue(":name", "Qt");
    
    //sql_query.bindValue(":id", 1);
    sql_query.bindValue(0, "Qt");
    sql_query.bindValue(1, 2);
    success = sql_query.exec();
    if(!success)
    {
        qDebug() << sql_query.lastError();
        return -1;
    }
    else
    {
        qDebug() << "updated!";
    }
    //#####################################################################
    //--查询数据
    //#####################################################################
    QString select_sql = "select id, name from student";
    success = sql_query.exec();
    if(!success)
    {
        qDebug()<<sql_query.lastError();
    }
    else
    {
        while(sql_query.next())
        {
            int id = sql_query.value(0).toInt();
            QString name = sql_query.value(1).toString();
            qDebug()<<QString("id:%1 name:%2").arg(id).arg(name);
        }
    }
    //查询最大的ID
    QString select_max_sql = "select max(id) from student";
    int max_id = 0;
    sql_query.prepare(select_max_sql);
    success = sql_query.exec();
    if(!success)
    {
   		 qDebug() << sql_query.lastError();
    }
    else
    {
        while(sql_query.next())
        {
            max_id = sql_query.value(0).toInt();
            qDebug() << QString("max id:%1").arg(max_id);
        }
    }
    //#####################################################################

    /--删除数据
    //#####################################################################
    QString delete_sql = "delete from student where id = ?";
    sql_query.prepare(delete_sql);
    sql_query.addBindValue(3);
    success = sql_query.exec();
    if(!success)
    {
        qDebug()<<sql_query.lastError();
        return -1;
    }
    else
    {
    	qDebug()<<"deleted!";
    }
    //清空表
    /*QString clear_sql = "delete from student";
    sql_query.prepare(clear_sql);
    success = sql_query.exec();
    if(!success)
    {
        qDebug() << sql_query.lastError();
        return -1;
    }
    else
    {
    	qDebug() << "table cleared";
    }*/
    //关闭数据库
    database.close();
    return a.exec();
}

```

