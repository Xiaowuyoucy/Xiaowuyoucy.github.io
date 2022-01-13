---
title: 进程回收函数
date: 2022-01-12 16:07:38
tags:
categories: linux
doc:
---



### wait函数

#### 头文件

```
#include <sys/types.h>
#include <sys/wait.h>
```



####  函数原型：

```
pid_t wait(int *status);
```

#### 函数作用：

阻塞并等待子进程退出 

回收子进程残留资源 

获取子进程结束状态(退出原因)。

#### 返回值：

成功：清理掉的子进程ID；

失败：-1 (没有子进程)

status参数：子进程的退出状态 -- 传出参数

```
WIFEXITED(status)：为非0    → 进程正常结束

WEXITSTATUS(status)：获取进程退出状态 

WIFSIGNALED(status)：为非0 → 进程异常终止

WTERMSIG(status)：取得进程终止的信号编号。
```

wait函数练习

使用wait函数完成父进程对子进程的回收

### waitpid函数

#### 头文件

```
#include <sys/types.h>
#include <sys/wait.h>
```



#### 函数原型：

```
pid_t waitpid(pid_t pid, int *status, in options);
```

#### 函数作用

同wait函数

#### 函数参数

参数：

pid：

pid = -1 等待任意一个子进程。与wait等效。

pid > 0 等待其进程ID与pid相等的子进程。等待指定的pid

pid = 0 等待进程组ID与目前进程相同的任何子进程，也就是说任何和调用

waitpid()函数的进程在同一个进程组的进程。

pid < -1 等待其组ID等于pid的绝对值的任一子进程。(适用于子进程在其他组的情况)

status: 子进程的退出状态，用法同wait函数。

options：设置为WNOHANG，函数非阻塞，设置为0，函数阻塞。

##### `调用一次wait或waitpid只能回收一个子进程`

#### 函数返回值

```
>0：返回回收掉的子进程ID；

-1：无子进程

=0：参3为WNOHANG，且子进程正在运行。
```

```c
//父进程调用wait函数完成对子进程的回收
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>

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
		int status;
		pid_t wpid = wait(&status);
		printf("wpid==[%d]\n", wpid);
		if(WIFEXITED(status)) //正常退出
		{
			printf("child normal exit, status==[%d]\n", WEXITSTATUS(status));
		}
		else if(WIFSIGNALED(status)) //被信号杀死
		{
			printf("child killed by signal, signo==[%d]\n", WTERMSIG(status));
		}
		
	}
	else if(pid==0) //子进程
	{
		
		printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
		sleep(20);
		return 9;
	}

	return 0;
}

```





```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>

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
		int status;
		//pid_t wpid = wait(&status);
        while(1){
          
	    pid_t wpid = waitpid(-1,&status,WNOHANG);
	
		//printf("wpid==[%d]\n", wpid);
        if(wpid > 0)
        {
          
	    	if(WIFEXITED(status)) //正常退出
	    	{
		    	printf("child normal exit, status==[%d]\n", WEXITSTATUS(status));
	    	}
	    	else if(WIFSIGNALED(status)) //被信号杀死
	    	{
		    	printf("child killed by signal, signo==[%d]\n", WTERMSIG(status));
	    	}
	
     }else if(wpid == 0) //子进程还活着
     {
        // printf("child is living,wpid[%d]\n",wpid);
     }else if(wpid == -1)
	 {
       printf("no child is living wpid[%d]\n",wpid);
       break;
     }
   
	}


	}
	else if(pid==0) //子进程
	{
		
		printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
		sleep(2);
		return 9;
	}

	return 0;
}

```



#### sleep函数

```
 #include <unistd.h>
```

```
sleep(5);	//表示睡眠5秒
```

