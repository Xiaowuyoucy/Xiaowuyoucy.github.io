---
title: fork函数
date: 2022-01-12 13:58:43
tags:
categories: linux
doc:
---

### 头文件

```
#include <sys/types.h>
#include <unistd.h>
```



#### 函数作用：

创建子进程

#### 原型: 

```
pid_t fork(void);
```

函数参数：无

#### 返回值：

调用成功:父进程返回子进程的PID，子进程返回0；

调用失败:返回-1，设置errno值。

####  fork函数代码片段实例

![img](/images/javawz/wps6E52.tmp.jpg) 

调用fork函数的内核实现原理:

![img](/images/javawz/wps6E53.tmp.jpg) 

### fork函数总结

#### fork函数的返回值？

 父进程返回子进程的PID，是一个大于0数;

 子进程返回0；

特别需要注意的是：不是fork函数在一个进程中返回2个值，而是在父子进程各自返回一个值。

#### 子进程创建成功后，代码的执行位置？

父进程执行到什么位置，子进程就从哪里执行

#### 如何区分父子进程

通过fork函数的返回值

父子进程的执行顺序

 不一定，哪个进程先抢到CPU，哪个进程就先执行

### getpid/getppid

#### 头文件

```
#include <sys/types.h>
#include <unistd.h>
```

getpid 得到当前进程的PID

```
pid_t getpid(void);
```

getppid得到当前进程的父进程的PID

```
pid_t getppid(void);
```



```c
//fork函数测试
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>

int main()
{
	printf("before fork, pid:[%d]\n", getpid());
	//创建子进程
	//pid_t fork(void);
	pid_t pid = fork();
	if(pid<0) //fork失败的情况
	{
		perror("fork error");
		return -1;
	}
	else if(pid>0)//父进程
	{
		printf("father: [%d], pid==[%d], fpid==[%d]\n", pid, getpid(),getppid());
		//sleep(1);
	}
	else if(pid==0) //子进程
	{
		printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
	}
	
	printf("after fork, pid:[%d]\n", getpid());

	return 0;
}

```



### 循环创建n个子程序

![image-20220112141606532](/images/javawz/image-20220112141606532.png)



```c
//循环创建n个子进程
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>

int main()
{
	int i = 0;
	for(i=0; i<3; i++)
	{
		//创建子进程
		pid_t pid = fork();
		if(pid<0) //fork失败的情况
		{
			perror("fork error");
			return -1;
		}
		else if(pid>0)//父进程
		{
			printf("father: pid==[%d], fpid==[%d]\n", getpid(),getppid());
			//sleep(1);
		}
		else if(pid==0) //子进程
		{
			printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
			break;
		}
	}

	//第1个子进程
	if(i==0)
	{
		printf("[%d]--[%d]: child\n", i, getpid());	
	}

	//第2个子进程
	if(i==1)
	{
		printf("[%d]--[%d]: child\n", i, getpid());	
	}
	//第3个子进程
	if(i==2)
	{
		printf("[%d]--[%d]: child\n", i, getpid());	
	}
	//父进程
	if(i==3)
	{
		printf("[%d]--[%d]: child\n", i, getpid());	
	}
	sleep(10);

	return 0;
}

```



### 验证父子进程能否共享全局变量

父子进程不能共享全局变量:

如果父子进程只是对全局变量做读操作,则父子进程在内存中只有一份，属于共享。但是如果父子进程中的如何一个进程对该全局变量做修改操作，会在内存中拷贝一个副本，然后在这个副本上进行修改，修改完成以后映射回去

写时复制，读时共享



![image-20220112141851453](/images/javawz/image-20220112141851453.png)



```c
//fork函数测试
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
int g_var = 99;

int main()
{
	//创建子进程
	pid_t pid = fork();
	if(pid<0) //fork失败的情况
	{
		perror("fork error");
		return -1;
	}
	else if(pid>0)//父进程
	{
		printf("father: [%d], pid==[%d], fpid==[%d]\n", pid, getpid(),getppid());
		g_var++;
		printf("[%p]", &g_var);
	}
	else if(pid==0) //子进程
	{
		sleep(1); //为了避免父进程还没有执行, 子进程已经结束了
		printf("[%p]", &g_var);
		printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
		printf("child: g_var==[%d]\n", g_var);
	}
	
	return 0;
}

```

