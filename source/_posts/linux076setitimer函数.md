---
title: setitimer函数
date: 2022-01-22 00:27:46
tags:
categories: linux
doc:
---



- **函数原型**    

```
int setitimer(int which, const struct itimerval *new_value,struct itimerval *old_value);
```

- **函数描述**

设置定时器(闹钟)，可代替alarm函数，精度微秒us，可以实现周期定时。

- **函数返回值**
  -  成功：0；
  -  失败：-1，设置errno值

-  **函数参数：** 
  -  which：指定定时方式
    - 自然定时：`ITIMER_REAL` → **14）SIGALRM**计算自然时间
    - 虚拟空间计时(用户空间)：`ITIMER_VIRTUAL `→ **26）SIGVTALRM** 只计算进程占用cpu的时间
    - 运行时计时(用户+内核)：`ITIMER_PROF` → **27）SIGPROF**计算占用cpu及执行系统调用的时间
  - new_value：`struct itimerval`, 负责设定timeout时间。
    - `itimerval.it_value`: 设定第一次执行function所延迟的秒数 
    - `itimerval.it_interval`: 设定以后每几秒执行function
  - old_value： 存放旧的timeout值，一般指定为NULL

```
struct itimerval { 

  struct timerval it_interval; 		// 闹钟触发周期

  struct timerval it_value; 		// 闹钟触发时间

 }; 

 struct timeval { 

  long tv_sec; 				// 秒

  long tv_usec; 			// 微秒

 }       
```



练习: 使用setitimer实现每隔一秒打印一次hello, world。

 

```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<sys/time.h>
#include<signal.h>

void sighandler(int signo)
{
    printf("hello world\n");
	printf("signo [%d]\n",signo);
}

int main(int argc,char * argv[])
{
	//注册捕获SIGALRM信号处理函数
	signal(SIGALRM,sighandler);
	
	struct itimerval tm;
	//周期性时间赋值
	tm.it_interval.tv_sec = 1;
	tm.it_interval.tv_usec = 0;
	//第一次触发的时间,3秒后每隔1秒发送一次SIGALRM信号
	tm.it_value.tv_sec = 3;
	tm.it_value.tv_usec = 0;
	
	
	setitimer(ITIMER_REAL,&tm,NULL);

	while(1)
	{
		sleep(1);
	}

	return 0;
}

```

