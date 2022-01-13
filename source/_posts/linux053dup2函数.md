---
title: dup2函数
date: 2022-01-10 18:12:57
tags:
categories: linux
doc:
---



![image-20220110181303574](/images/javawz/image-20220110181303574.png)



快速生成可执行文件

```
make 源文件名(去掉.c后缀)
例如当前目录下有一个main.c文件
make main	//会生成一个main可执行文件

```

### dup2函数

#### 头文件

```
#include <unistd.h>
```

函数描述: 复制文件描述符

#### 函数原型:

```
int dup2(int oldfd, int newfd);
```

#### 函数参数: 

`oldfd-`原来的文件描述符

`newfd-`复制成的新的文件描述符

#### 函数返回值:

成功: 将oldfd复制给newfd, 两个文件描述符指向同一个文件

失败: 返回-1, 设置errno值

假设newfd已经指向了一个文件，首先close原来打开的文件，然后newfd指向oldfd指向的文件.

若newfd没有被占用，newfd指向oldfd指向的文件.



#### dup2函数实现重定向操作

```
//使用dup2函数实现标准输出重定向操作
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main(int argc, char *argv[])
{
	//打开文件
	int fd = open(argv[1], O_RDWR | O_CREAT, 0777);
	if(fd<0)
	{
		perror("open error");
		return -1;
	}

 	//调用dup2函数实现文件重定向操作
	dup2(fd, STDOUT_FILENO);
		
	printf("ni hao hello world");

	close(fd);
	close(STDOUT_FILENO);

	return 0;
}

```

![image-20220111004510141](/images/javawz/image-20220111004510141.png)