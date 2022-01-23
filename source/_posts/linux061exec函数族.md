---
title: exec函数族
date: 2022-01-12 15:02:22
tags:
categories: linux
doc:
---

#### 函数作用和函数介绍   

  有的时候需要在一个进程里面执行其他的命令或者是用户自定义的应用程序，此时就用到了exec函数族当中的函数。

使用方法一般都是在父进程里面调用fork创建处子进程，然后在子进程里面调用exec函数。

### execl函数

```
#include <unistd.h>
```

函数原型: 

```
int execl(const char *path, const char *arg, ... /* (char  *) NULL */);
```

参数介绍：

path: 要执行的程序的绝对路径

变参arg: 要执行的程序的需要的参数

arg:占位，通常写应用程序的名字

arg后面的: 命令的参数

参数写完之后: NULL

返回值：若是成功，则不返回，不会再执行exec函数后面的代码；若是失败，会执行execl后面的代码，可以用perror打印错误原因。

execl函数一般执行自己写的程序。

 

#### execlp函数

```
#include <unistd.h>
```

函数原型: 

```
int execlp(const char *file, const char *arg, .../* (char  *) NULL */);
```

参数介绍：

file: 执行命令的名字, 根据PATH环境变量来搜索该命令

arg:占位

arg后面的: 命令的参数

参数写完之后: NULL

返回值：若是成功，则不返回，不会再执行exec函数后面的代码；若是失败，会执行exec后面的代码，可以用perror打印错误原因。

execlp函数一般是执行系统自带的程序或者是命令.

#### exec函数族原理介绍  

exec族函数的实现原理图：

 如：

```
execlp(“ls”, “ls”, “-l”, NULL);
```

![image-20220112151630526](/images/javawz/image-20220112151630526.png)

总结：

exec函数是用一个新程序替换了当前进程的代码段、数据段、堆和栈；原有的进程空间没有发生变化，并没有创建新的进程，进程PID没有发生变化。



如果想在一个进程内部执行系统命令或者是应用程序, 优先应该想到如下方式:

先fork(), 然后在子进程里面执行execl拉起可执行程序或者命令.

```
pid = fork();
if(pid==0)
{
	execl(...);
}
```

execl: 一般用于执行用户自定义的应用程序.
execlp: 一般用于执行系统命令

```c
//fork函数测试
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>

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
		//sleep(1);
	}
	else if(pid==0) //子进程
	{
		printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
		//execl("/bin/ls", "ls", "-l", NULL);
		//execl("./test", "test", "hello", "world", "ni", "hao", NULL);
		//execlp("ls", "ls", "-l", NULL);
		execlp("./test", "TESTING", "hello", "world", "ni", "hao", NULL);
		//execlp("./iitest", "test", "hello", "world", "ni", "hao", NULL);
		perror("execl error");
	}
	
	return 0;
}

```

