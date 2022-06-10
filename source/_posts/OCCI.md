---
title: OCCI
date: 2022-06-05 08:38:09
tags:
categories: 多端安全协议传输平台
doc:
---

### 编写服务端程序

服务端使用多线程处理多个客户端连接, 一个线程处理一个客户端连接, 一个客户端需要使用一个TcpSocket对象,
可以使用map完成线程ID和TcpSocket的映射关系, map<pthread_t, TcpSocket *>m_listSocket;

线程的回调函数需要访问到服务端操作类的成员变量, 所以应该将回调函数设置为类的友元函数.

```
auto it = m_socketList.find(threadId);---->自动类型推导
但是如果it是先定义后赋值不行:
auto it;
it = m_socketList.find(threadId);
```

在使用SHA1进行加密的时候, 函数参数是什么类型, 就提供什么类型的数据, 否则可能会有问题.

### 守护进程

1. 守护进程特点

>- 后台服务进程
>- 独立于控制终端
>- 周期性执行某任务
>- 不受用户登录注销影响
>- 一般采用以d结尾的名字(服务)

2. 创建守护进程的流程

   - 创建子进程, 杀死父进程 - 必须的
     - 创建子进程: fork();
     - 守护进程是怎么来的?
       - 有一个没有任何职务的进程才可以被加官进爵, 守护进程是一个会话
         - 范围: 进程 -> 进程组 -> 会话
           - 进程组中的组长: 默认是进程组中的第一个进程
     - 如何杀死父进程
       - exit(0)
       - kill()
       - raise() -> 自己给自己发信号
       - abort() -> 给自己发送SIGABRT信号
   - 子进程提升为会话 - 必须的
     - setsid()
     - 提升成功之后, 进程就脱离了终端
   - 修改工作目录 - 可选
     - chdir();
     - /mnt/U盘1/app
       - 在当前目录/mnt/U盘1/下 执行 ./app, 进程的工作路径: /mnt/U盘1/
     - 将工作目录切换到一个不能被卸载的路径下就可以了
       - chdir("/home");
   - 修改掩码 - 可选
     - umask();
   - 关闭/重定向文件描述法 - 可选
     - 标准输入 -> 0
     - 标准输出 -> 1
     - 标准错误 -> 2
     - 如果的重定向, 对应的位置: /dev/null
       - /dev/null是一个黑洞文件,不管放什么数据进去,文件大小都是0
       - 重定向的目的: 排除干扰

   

   ```
   int devFd = open("/dev/null", O_RDWR);
   int dup2(int oldfd, int newfd);
   dup2(devFd, 0);
   newfd 不管之前指向哪一个文件, 现在指向oldfd
   ```

   

   

   - 核心处理动作 -> 周期性的执行某些操作 - 必须
   - 关闭守护进程 -> 使用shell脚本管理 - 可选

## OCCI

> Oracle C++调用接口 -- OCCI 即 Oracle C++ Call Interface
> OCCI 是Oracle 的C++ API, 允许你使用面向对象的特性、本地类、C++语言的方法来访问Oracle数据库

### OCCI 介绍

1. 优势

   > 1. 基于标准C++和面向对象的设计；
   > 2. 效率较高；
   > 3. 适合开发C/S模式的程序，软件中间层；

2. 特性

   > 1. 完整支持SQL/PLSQL
   > 2. 为不断增长的用户和请求提供了弹性选项
   > 3. 为使用用户自定义类型，如C中的类，提供了无缝接口
   > 4. 支持所有的Oracle数据类型以及LOB types（大对象）
   > 5. 可以访问数据库元数据

OCCI 头文件

```
#include <occi.h> -- 程序中只需要包含这一个头文件
#include <occiCommon.h>
#include <occiControl.h>
#include <occiData.h>
#include <occiObjects.h>
```

4. OCCI 库

- Windows
  - oraocci11.lib/ oraocci11d.lib
  - oraocci11.dll/ oraocci11d.dll

- Linux
  - libnnz11.so
  - libocci.so
  - libclntsh.so

linux下的环境配置-----root用户下进行配置

- 将oracle_client_11gR2.tar.gz文件上传值linux操作系统的/opt目录下
- 执行tar -zxvf oracle_client_11gR2.tar.gz解压至当前目录下
- 进入到刚刚解压的目录, 打开<<Hi-看我,看我.sh>>
- 将文件中的export导出的环境变量拷贝到root用户的.bashrc文件中
  - 注意: 若解压的目录不是/opt, 环境变量中的路径需要修改
- 执行. .bashrc或者source .bashrc 或者退出再次登录使配置的环境变量生效
  - 可以执行echo $OCCI_HOME进行查看, 若看到内容则设置成功
- 切换到/opt/instantclient_11_2/network/admin目录下
  - 打开tnsnames.ora文件, 修改其中的HOST部分, 将IP修改成实际的oracle服务的IP地址
- occi.cpp测试代码上传到root用户下
  - 然后执行: g++ -o a.out occi.cpp -locci -lclntsh, 编译通过表明设置的没有问题.
  - 若执行报错, 查看一下代码中的oracle的用户名和密码是否正确.



常见的几个环境变量:

```
PATH:命令或者可执行程序搜索的路径

C_INCLUDE_PATH:gcc编译器查找头文件的路径

CPLUS_INCLUDE_PATH: g++编译器查找头文件的路径

LD_LIBRARY_PATH:查找动态链接库的路径

LIBRARY_PATH: 查找静态库的路径

若编译的时候使用静态库文件:
则: gcc foo.c -L /home/itcast/lib -static -lfoo -o foo
```





```
oracle用户安装了oracle服务系统, 本身就有oracle编程需要的库文件和头文件:
> export OCCI_HOME=/u01/app/oracle/product/11.2.0/db_1
> export OCCI_INCLUDE_DIR=$OCCI_HOME/rdbms/public
> export OCCI_LIBRARY_PATH=$OCCI_HOME/lib
> export LD_LIBRARY_PATH=$$LD_LIBRARY_PATH:$OCCI_LIBRARY_PATH
>
> 程序编译时搜索的库目录
>
> export LIBRARY_PATH=$$LIBRARY_PATH:$OCCI_LIBRARY_PATH
>
> 程序编译时搜索的头文件目录
>
> export CPLUS_INCLUDE_PATH=$$CPLUS_INCLUDE_PATH:$OCCI_INCLUDE_DIR
>
> 上面的环境变量中OCCI_HOME和OCCI_INCLUDE_DIR与在root中的配置不同, 其余相同
```





## OCCI使用



### 初始化 - Environment 类

> OCCI通过创建一个Environment的对象完成初始化工作。
> 可以通过Environment创建数据库连接，从而进行其它的操作
> 要创建Environment，应该调用Environment类的静态方法createEnvironment()



```
// 环境初始化
Environment* env = Environment::createEnvironment();
// 关闭, 释放资源
Environment::terminateEnvironment(env);
```



### 连接数据库 - Connection 类

>连接数据库通过Connection类的对象实例实现
>
>调用Environment类的createConnection()方法可以创建一个Connection对象；



```
// 函数原型:
Connection *Environment::createConnection(
const string &userName,const string &password, const string &connectString );
```



```
// 函数调用
const string userName = "scott"; // 用户名
const string passwd = "tiger"; // 密码
const string connstr = "192.168.247.129:1521/orcl"; // 数据库连接串
Connection* conn = env->createConnection(userName, passwd, connstr);
```



> 使用Environment::terminateConnection()断开连接

```
// 断开连接
env->terminateConnection(conn);
```



### 执行SQL

>Statement 类用于执行SQL语句,并获取返回结果。
>ResultSet 类用于处理SELECT 查询的结果。
>对于所有类型的数据的绑定或者获取，OCCI都提供了统一的方法
>setXXX 方法用于Statement
>getXXX 方法用于Statement & ResultSet
>OCCI会自动处理类型之间的转换。
>使用方法：
>使用Connection::createStatement()创建Statement对象, 指定 SQL 命令(DDL/DML/query)作为参数



```
// 操作函数
Connection::createStatement(string &sql);

Statement::setSQL(string &sql);

Statement::execute(string &sql); // can be used for any SQL, returns status

Statement::executeUpdate(string &sql); // returns Insert/Update/Delete count

Statement::executeQuery(string &sql); // returns ResultSet（结果集）
```



>使用 setXXX 方法传递要绑定用于输入的值
>使用合适的execute方法执行SQL
>对于SELECT 查询, 使用ResultSet 对象处理返回结果

```
// 插入操作
Statement *stmt = conn->createStatement(“ insert into Dept(Deptno,Dname, Loc) values (1,
‘ACCOUNTS’, ‘ZONE1’ ”);
stmt->executeUpdate();
conn->terminateStatement(stmt); // 关闭查询, 释放资源
```



>使用绑定参数的DML（数据操作语句）示例：

```
Statement *stmt = conn->createStatement(“ insert into Emp(EmpNo,Ename) values(:1, :2) ”);
//1 and 2 are bind placeholders
int empno = 2;
string empname = “JOHN W”;
//first parameter is bind position, second is value
stmt->setInt(1, empno);
stmt->setString(2, empname);
stmt->executeUpdate();
```

> 执行SELECT查询并处理结果：

```
Statement *stmt = conn->createStatement(
	“select Empno, Ename, Sal from Emp where Hiredate >= :1”);
//automatically converted to Date
stmt->setString(1, “01-JAN-1987”);
//executeQuery returns a ResultSet
ResultSet *rs = stmt->executeQuery();
//ResultSet::next fetches rows and returns FALSE
//when no more rows
while (rs->next() == true)
{
    //get values using the getXXX methods of ResultSet
    empno = rs->getInt(1);
    empname = rs->getString(2);
    empsalary = rs->getFloat(3);
}
stmt->closeResultSet(rs);//to free resources
```



## window配置occi环境

包含头文件和库路径还有库名字

![image-20220605190706609](/images/javawz/image-20220605190706609.png)

### 1. Linux如何查找动态库(编译、运行) ?

1. 编译和链接时，动态库的搜索路径顺序如下（注意不会递归性地在其子目录下搜索）：

```
(1) gcc编译、链接命令中的-L选项；
(2) gcc的环境变量的 LIBRARY_PATH（多个路径用冒号分割）；
(3) gcc默认动态库目录：/lib:/usr/lib:usr/lib64:/usr/local/lib。
```

2. 运行时，动态库的搜索路径顺序如下：

```
(1) 编译目标代码时指定的动态库搜索路径：用选项-Wl,rpath和include指定的动态库的搜索路径，
	比如gcc -Wl,-rpath,include -L. -ldltest hello.c，在执行文件时会搜索路径`./include`;
	
(2) 环境变量LD_LIBRARY_PATH（多个路径用冒号分割）;

(3) 修改 /etc/ld.so.conf 配置文件指定的动态库绝对路径, /etc/ld.so.cache
（通过sudo ldconfig生效，一般是非root用户时使用）;

(4) gcc默认动态库目录：/lib:/usr/lib:usr/lib64:/usr/local/lib 等。
```



### 2. oracle解锁用户

```
// 解锁用户
ALTER USER scott ACCOUNT UNLOCK;
// 重写设置密码
ALTER USER scott IDENTIFIED BY tiger;
```





























