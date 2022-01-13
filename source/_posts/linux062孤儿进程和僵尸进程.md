---
title: 孤儿进程和僵尸进程
date: 2022-01-12 15:24:59
tags:
categories: linux
doc:
---

#### 为什么要进行进程资源的回收

  当一个进程退出之后，进程能够回收自己的用户区的资源，但是不能回收内核空间的PCB资源，必须由它的父进程调用wait或者waitpid函数完成对子进程的回收，避免造成系统资源的浪费。

###  孤儿进程

#### 孤儿进程的概念：

若子进程的父进程已经死掉，而子进程还存活着，这个进程就成了孤儿进程。

为了保证每个进程都有一个父进程，孤儿进程会被init进程领养，init进程成为了孤儿进程的养父进程，当孤儿进程退出之后，由init进程完成对孤儿进程的回收。

#### 模拟孤儿进程的案例

编写模拟孤儿进程的代码讲解孤儿进程，验证孤儿进程的父进程是否由原来的父进程变成了init进程。

### 僵尸进程

#### 僵尸进程的概念:

若子进程死了，父进程还活着， 但是父进程没有调用wait或waitpid函数完成对子进程的回收，则该子进程就成了僵尸进程。

#### 如何解决僵尸进程

由于僵尸进程是一个已经死亡的进程，所以不能使用kill命令将其杀死

通过杀死其父进程的方法可以消除僵尸进程。

杀死其父进程后，这个僵尸进程会被init进程领养，由init进程完成对僵尸进程的回收。

#### 模拟僵尸进程的案例


	

```c
//孤儿进程
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
		sleep(5);
		printf("father: [%d], pid==[%d], fpid==[%d]\n", pid, getpid(),getppid());
	}
	else if(pid==0) //子进程
	{
		
		printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
		sleep(20);
		printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
	}
	

	return 0;
}

```

```c
//僵尸进程
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
		sleep(100);
		printf("father: [%d], pid==[%d], fpid==[%d]\n", pid, getpid(),getppid());
	}
	else if(pid==0) //子进程
	{
		
		printf("child: pid==[%d], fpid==[%d]\n", getpid(), getppid());
	}
	

	return 0;
}

```

编写模拟僵尸进程的代码讲解僵尸进程, 验证若子进程先于父进程退出, 而父进程没有调用wait或者waitpid函数进行回收, 从而使子进程成为了僵尸进程.

孤儿进程: 父进程先退出, 子进程就变成了孤儿进程, 此时被init进程领养,
          当孤儿进程退出之后, 就会被init进程回收.

僵尸进程: 子进程先退出, 父进程没有完成对子进程的回收, 此时子进程就变成了僵尸进程.
如何解决僵尸进程:
	不能使用kill -9杀死僵尸进程, 原因是僵尸进程是一个死掉的进程;
	应该使用杀死僵尸进程父进程的方法来解决僵尸进程;
	原因是: 杀死其父进程可以让init进程领养僵尸进程,最后由init进程回收僵尸进程.

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>