---
title: sigaction函数
date: 2022-01-22 21:10:27
tags:
categories: linux
doc:
---

-  sigaction函数
  -  函数说明：注册一个信号处理函数
  -  函数原型：

  ```
   int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
  ```
  -  函数参数：
  - signum：捕捉的信号
  - act：  传入参数，新的处理方式。
  - oldact： 传出参数，旧的处理方式

 

```
struct sigaction {

void  (*sa_handler)(int);	// 信号处理函数

void  (*sa_sigaction)(int, siginfo_t *, void *); //信号处理函数

sigset_t  sa_mask; //信号处理函数执行期间需要阻塞的信号

int    sa_flags; //通常为0，表示使用默认标识

void   (*sa_restorer)(void);

};
```

 

- 总结：
  -  sa_handler：指定信号捕捉后的处理函数名(即注册函数)。也可赋值为SIG_IGN表忽略 或 SIG_DFL表执行默认动作
  -  sa_mask: 用来指定在信号处理函数执行期间需要被屏蔽的信号，特别是当某个信号被处理时，它自身会被自动放入进程的信号掩码，因此在信号处理函数执行期间这个信号不会再度发生。注意：仅在处理函数被调用期间屏蔽生效，是临时性设置。
  -  sa_flags：通常设置为0，使用默认属性。
  -  sa_restorer：已不再使用	

 

-  练习：编写程序，使用sigaction函数注册信号捕捉函数，并使用这个程序验证信号是否支持排队。



```c
//sigaction函数测试---注册信号处理函数
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
#include <signal.h>

//信号处理函数
void sighandler(int signo)
{
	printf("signo==[%d]\n", signo);
	sleep(4);
}

int main()
{
	//注册信号处理函数
	struct sigaction act;
	act.sa_handler = sighandler;
	sigemptyset(&act.sa_mask);  //在信号处理函数执行期间, 不阻塞任何信号
	sigaddset(&act.sa_mask, SIGQUIT);
	act.sa_flags = 0;
	sigaction(SIGINT, &act, NULL);

	
	signal(SIGQUIT, sighandler);	

	while(1)
	{
		sleep(10);
	}

	return 0;
}

```



-  知识点: 信号处理不支持排队:
  -  在XXX信号处理函数执行期间, XXX信号是被阻塞的, 如果该信号产生了多次, 在XXX信号处理函数结束之后,  该XXX信号只被处理一次.
  - 在XXX信号处理函数执行期间,如果阻塞了YYY信号, 若YYY信号产生了多次, 当XXX信号处理函数结束后, YYY信号只会被处理一次.

-  内核实现信号捕捉的过程

如果信号的处理动作是用户自定义函数，在信号递达时就调用这个函数，这称为捕捉信号。由于信号处理函数的代码是在用户空间的，处理过程比较复杂，举例如下：

用户程序注册了SIGQUIT信号的处理函数sighandler。

当前正在执行main函数，这时发生中断或异常切换到内核态。

在中断处理完毕后要返回用户态的main函数之前检查到有信号SIGQUIT递达。

内核决定返回用户态后不是恢复main函数的上下文继续执行，而是执行sighandler函数，sighandler和main函数使用不同的堆栈空间，它们之间不存在调用和被调用的关系，是两个独立的控制流程。

sighandler函数返回后自动执行特殊的系统调用sigreturn再次进入内核态。

如果没有新的信号要递达，这次再返回用户态就是恢复main函数的上下文继续执行了。

 

![img](/images/javawz/wps7CFB.tmp.jpg) 

 