---
title: 管道的读写行为
date: 2022-01-17 01:54:45
tags:
categories: linux
doc:
---

### 读操作

#### 有数据

read正常读，返回读出的字节数

#### 无数据

1.写端全部关闭

read解除阻塞，返回0, 相当于读文件读到了尾部

2.没有全部关闭

read阻塞

### 写操作

#### 读端全部关闭

管道破裂，进程终止, 内核给当前进程发SIGPIPE信号

#### 读端没全部关闭

1.缓冲区写满了

write阻塞

2.缓冲区没有满

继续write



```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>
#include <fcntl.h>

int main()
{
	//创建管道
	//int pipe(int pipefd[2]);
	int fd[2];
	int ret = pipe(fd);
	if(ret<0)
	{
		perror("pipe error");
		return -1;
	}
	printf("pipe size==[%ld]\n", fpathconf(fd[0], _PC_PIPE_BUF));
	printf("pipe size==[%ld]\n", fpathconf(fd[1], _PC_PIPE_BUF));

	close(fd[0]);
    while(1)
    {
        write(fd[1], "hello world", strlen("hello world"));	
    }
	

	//关闭写端
	close(fd[1]);

	char buf[64];
	memset(buf, 0x00, sizeof(buf));
	int n = read(fd[0], buf, sizeof(buf));
	printf("read over, n==[%d], buf==[%s]\n", n, buf);

	return 0;
}

```

