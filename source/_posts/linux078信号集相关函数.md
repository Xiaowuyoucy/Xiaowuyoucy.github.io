---
title: 信号集相关函数
date: 2022-01-22 14:35:35
tags:
categories: linux
doc:
---

由于信号集属于内核的一块区域，用户不能直接操作内核空间，为此，内核提供了一些信号集相关的接口函数，使用这些函数用户就可以完成对信号集的相关操作。

信号集是一个能表示多个信号的数据类型，`sigset_t set`，set即一个信号集。既然是一个集合，就需要对集进行添加、删除等操作。

```
sigset_t类型的定义在signal.h文件中的第49行处:
typedef __sigset_t sigset_t;
__sigset_t的定义在sigset.h文件中的26，27行处: 
# define _SIGSET_NWORDS (1024 / (8 * sizeof (unsigned long int)))

  typedef struct
  {
    unsigned long int __val[_SIGSET_NWORDS];
  } __sigset_t;
  

上述变量类型的定义的查找有个小窍门： 可以执行gcc的预处理命令：
gcc -E test.c -o test.i 这样头文件就会展开，可以直接到test.i文件中看到相关变量类型的定义。
```

信号集相关函数

- ` int sigemptyset(sigset_t *set);`

函数说明：将某个信号集清0		 

函数返回值：成功：0；失败：-1，设置errno

- ` int sigfillset(sigset_t *set);`

函数说明：将某个信号集置1		 		

函数返回值：成功：0；失败：-1，设置errno

- `int sigaddset(sigset_t *set, int signum);	` 

函数说明：将某个信号加入信号集合中

函数返回值：成功：0；失败：-1，设置errno

- ` int sigdelset(sigset_t *set, int signum);		`

函数说明：将某信号从信号清出信号集  	

函数返回值：成功：0；失败：-1，设置errno

-  `int sigismember(const sigset_t *set, int signum);`

函数说明：判断某个信号是否在信号集中

函数返回值：在：1；不在：0；出错：-1，设置errno

-  sigprocmask函数

函数说明：用来屏蔽信号、解除屏蔽也使用该函数。其本质，读

取或修改进程控制块中的信号屏蔽字（阻塞信号集）。

特别注意，屏蔽信号只是将信号处理延后执行(延至解除屏蔽)；而忽略表示将信号丢弃处理。

函数原型：`int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);`

函数返回值：成功：0；失败：-1，设置errno

函数参数：

how参数取值：假设当前的信号屏蔽字为mask

`SIG_BLOCK`: 当how设置为此值，set表示需要屏蔽的信号。**相当于** mask = mask | set

`SIG_UNBLOCK`: 当how设置为此，set表示需要解除屏蔽的信号。相当于 mask = mask & ~set

`SIG_SETMASK`: 当how设置为此，set表示用于替代原始屏蔽及的新屏蔽集。相当于mask = set若，调用sigprocmask解除了对当前若干个信号的阻塞，则在sigprocmask返回前，至少将其中一个信号递达。

set：传入参数，是一个自定义信号集合。由参数how来指示如何修改当前信号屏蔽字。

oldset：传出参数，保存旧的信号屏蔽字。

-  sigpending函数

函数原型：`int sigpending(sigset_t *set);	  `

函数说明：读取当前进程的未决信号集

函数参数：set传出参数

函数返回值：成功：0；失败：-1，设置errno



练习：编写程序，设置阻塞信号集并把所有常规信号的未决状态打印至屏幕。

```c
//信号集相关函数测试
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
#include <signal.h>

//信号处理函数
void sighandler(int signo)
{
	printf("signo==[%d]\n", signo);
}

int main()
{

	//注册SIGINT和SIGQUIT的信号处理函数
	signal(SIGINT, sighandler);
	signal(SIGQUIT, sighandler);

	//定义sigset_t类型的变量
	sigset_t pending, mask, oldmask;

	//初始化
	sigemptyset(&pending);
	sigemptyset(&mask);
	sigemptyset(&oldmask);

	//将SIGINT和SIGQUIT加入到阻塞信号集中
	sigaddset(&mask, SIGINT);
	sigaddset(&mask, SIGQUIT);

	//将mask中的SIGINT和SIGQUIT信号加入到阻塞信号集中
	//sigprocmask(SIG_BLOCK, &mask, NULL);
	sigprocmask(SIG_BLOCK, &mask, &oldmask);

	int i = 1;
	int k = 1;
	while(1)
	{
		//获取未决信号集
		sigpending(&pending);	

		for(i=1; i<32; i++)
		{
			//判断某个信号是否在集合中
			if(sigismember(&pending, i)==1)	
			{
				printf("1");
			}
			else
			{
				printf("0");	
			}
		}
		printf("\n");

		if(k++%10==0)
		{
			//从阻塞信号集中解除对SIGINT和SIGQUIT的阻塞
			//sigprocmask(SIG_UNBLOCK, &mask, NULL);	
			sigprocmask(SIG_SETMASK, &oldmask, NULL);	
		}
		else
		{
			sigprocmask(SIG_BLOCK, &mask, NULL);	
		}

		sleep(1);
	}

	return 0;
}

```

