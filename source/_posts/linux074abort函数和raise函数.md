---
title: abort函数和raise函数
date: 2022-01-20 20:16:22
tags:
categories: linux
doc:
---

- raise函数
  -  函说描述：给当前进程发送指定信号(自己给自己发)	

  -  函数原型：

    ```
    int raise(int sig);
    ```

  -  函数返回值：成功：0，失败非0值

  -  函数拓展:

```
raise(signo) == kill(getpid(), signo);
```



-  abort函数

  -  函数描述：给自己发送异常终止信号 **6) SIGABRT**，并产生core文件
  -  函数原型：`void abort(void);  `

  - 设置core文件大小

    ```
    ulimit -c unlimited			//无限制大小
    ulimit -x	//x代表的是 下面带-的参数,比如-c
    yxc19980620c@yc:~/my/demo05$ ulimit -a
    core file size          (blocks, -c) 0
    data seg size           (kbytes, -d) unlimited
    scheduling priority             (-e) 0
    file size               (blocks, -f) unlimited
    pending signals                 (-i) 15434
    max locked memory       (kbytes, -l) 65536
    max memory size         (kbytes, -m) unlimited
    open files                      (-n) 1024
    pipe size            (512 bytes, -p) 8
    POSIX message queues     (bytes, -q) 819200
    real-time priority              (-r) 0
    stack size              (kbytes, -s) 8192
    cpu time               (seconds, -t) unlimited
    max user processes              (-u) 15434
    virtual memory          (kbytes, -v) unlimited
    file locks                      (-x) unlimited
    
    ```

    

  - 函数拓展：

    ```
    abort() == kill(getpid(), SIGABRT);
    ```



```c
//raise和abort函数测-
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

	//给当前进程发送SIGINT信号
	raise(SIGINT);

	//给当前进程发送SIGABRT
	abort();

	while(1)
	{
		sleep(10);
	}

	return 0;
}

```

