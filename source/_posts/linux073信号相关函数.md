---
title: 信号相关函数
date: 2022-01-20 18:38:02
tags:
categories: linux
doc:
---

### signal函数

- 函数作用：注册信号捕捉函数

- 函数原型

```
typedef void (*sighandler_t)(int);

sighandler_t signal(int signum, sighandler_t handler);
```

- 函数参数
  - signum：信号编号
  - handler：信号处理函数

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
  
  	//while(1)
  	{
  		sleep(10);
  	}
  
  	return 0;
  }
  
  ```

  

### kill函数/命令

- 描述：给指定进程发送指定信号

- kill命令：kill -SIGKILL 进程PID

- kill函数原型：

  ```
  int kill(pid_t pid, int sig);	
  ```

  

- 函数返回值：
  -  成功：0；
  -  失败：-1，设置errno

- 函数参数：
  - sig信号参数：不推荐直接使用数字，应使用宏名，因为不同操作系统信号编号可能不同，但名称一致。
  -  pid参数：
    -  pid > 0: 发送信号给指定的进程。
    -  pid = 0: 发送信号给与调用kill函数进程属于同一进程组的所有进程。
    -  pid < -1:  取|pid|发给对应进程组。
    -  pid = -1：发送给进程有权限发送的系统中所有进程。

进程组：每个进程都属于一个进程组，进程组是一个或多个进程集合，他们相互关联，共同完成一个实体任务，每个进程组都有一个进程组长，默认进程组ID与进程组长ID相同。

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


	while(1)
	{
		sleep(1);
		kill(getpid(), SIGINT);
	}

	return 0;
}

```



<br /><br /><br /><br /><br /><br /><br /><br />