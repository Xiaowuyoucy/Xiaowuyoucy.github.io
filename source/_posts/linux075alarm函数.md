---
title: alarm函数
date: 2022-01-21 20:20:15
tags:
categories: linux
doc:
---

### alarm函数

- 函数原型：`unsigned int alarm(unsigned int seconds); `

- 函数描述：设置定时器(闹钟)。在指定seconds后，内核会给当前进程发送14）SIGALRM信号。进程收到该信号，默认动作终止。每个进程都有且只有唯一的一个定时器。

- 函数返回值：返回0或剩余的秒数，无失败。例如：

![img](/images/javawz/wps5652.tmp.jpg) 

- 常用操作：取消定时器alarm(0)，返回旧闹钟余下秒数。

alarm使用的是自然定时法，与进程状态无关，就绪、运行、挂起(阻塞、暂停)、终止、僵尸...无论进程处于何种状态，alarm都计时。 

<br>

=============================================================================

<br>

练习题1：编写一个程序测试alarm函数

练习题2：编写程序，测试你的电脑1秒种能数多个数字。

- 使用time命令查看程序执行的时间。程序运行的瓶颈在于IO，优化程序，首选优化IO。

- 实际执行时间 = 系统时间 + 用户时间 + 损耗时间

损耗的时间主要来来自文件IO操作，IO操作会有用户区到内核区的切换，切换的次数越多越耗时。

```c
//signal函数测试---注册信号处理函数
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
	//注册信号处理函数
	signal(SIGINT, sighandler);
	signal(SIGALRM, sighandler);

	int n = alarm(5);
	printf("first: n==[%d]\n", n);

	sleep(2);
	n = alarm(5);
	//n = alarm(0); //取消时钟
	printf("second: n==[%d]\n", n);

	while(1)
	{
		sleep(10);
	}

	return 0;
}

```



```c
//测试1秒钟可以数多少数字
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
#include <signal.h>

int main()
{
    //一秒钟后发送信号
	alarm(1);
	int i = 0;
	while(1)
	{
		printf("[%d]", i++);
	}

	return 0;
}

```



### time命令

查看程序使用的时间

```
time 程序名

real   实际执行时间
user    用户时间
sys     系统时间(内核时间)
```

实际执行时间 = 系统时间 + 用户时间 + 损耗时间
损耗时间= 实际执行时间-(系统时间 + 用户时间 )
每一个数字都直接打印:`printf("[%d]\n", i++);`

```
real    0m1.217s
user    0m0.120s
sys     0m0.252s
15734次
```

`损耗时间= 1.217-(0.120+0.252)=0.845`
文件重定向之后:

```
time ./alarm_uncle  > test.log
real    0m1.003s
user    0m0.520s
sys     0m0.428s
2191879次
```

`损耗时间=1.003-(0.520+0.428)=0.055`
原因是: 调用`printf`函数打印数字遇到`\n`才会打印, 打印过程涉及到从
用户区到内核区的切换, 切换次数越多消耗的时间越长, 效率越低;
而使用文件重定向, 由于文件操作是带缓冲的, 所以涉及到用户区到内核区的
切换次数大大减少,从而使损耗降低.