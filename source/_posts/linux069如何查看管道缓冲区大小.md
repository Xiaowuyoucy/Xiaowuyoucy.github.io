---
title: 如何查看管道缓冲区大小
date: 2022-01-17 18:58:01
tags:
categories: linux
doc:
---

###  命令

```
ulimit -a
```



### 函数 

```
long fpathconf(int fd, int name);

printf("pipe size==[%ld]\n", fpathconf(fd[0], _PC_PIPE_BUF));

printf("pipe size==[%ld]\n", fpathconf(fd[1], _PC_PIPE_BUF));
```



```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<unistd.h>
#include<string.h>
#include<wait.h>

int main(int argc,char *argv[])
{
	int fd[2] = {0};
	int ret = pipe(fd);

	if(ret == -1){
		perror("pipe error");
		exit(-1);
	}
    //获取管道缓冲区大小
	printf("pipe size [%ld]\n",fpathconf(fd[0],_PC_PIPE_BUF));
	pid_t pid = fork();
	if(pid == -1)
	{
		perror("fork error");
		exit(-1);
	}
	
	if(pid > 0)
	{
		close(fd[0]);
		write(fd[1],"hello pipe!",sizeof("hello pipe!"));
		close(fd[1]);
		wait(NULL);
	}
	if(pid == 0){
		char buf[1024];
		close(fd[1]);
		read(fd[0],buf,1024);
		printf("%s\n",buf);
	}
	return 0;
}


```

