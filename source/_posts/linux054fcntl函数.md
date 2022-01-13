---
title: fcntl函数
date: 2022-01-11 01:35:43
tags:
categories: linux
doc:
---



#### fcntl函数

函数描述: 改变已经打开的文件的属性

#### 函数原型: 

```
int fcntl(int fd, int cmd, ... /* arg */ );
```

若cmd为F_DUPFD, 复制文件描述符, 与dup相同

若cmd为F_GETFL, 获取文件描述符的flag属性值

若cmd为 F_SETFL, 设置文件描述符的flag属性

#### 函数返回值:返回值取决于cmd

成功:

若cmd为F_DUPFD, 返回一个新的文件描述符

若cmd为F_GETFL, 返回文件描述符的flags值

若cmd为 F_SETFL, 返回0

失败返回-1, 并设置errno值.

 

### fcntl函数常用的操作:

#### 1.复制一个新的文件描述符:

```
int newfd = fcntl(fd, F_DUPFD, 0);
```

#### 2.获取文件的属性标志

```
int flag = fcntl(fd, F_GETFL, 0)
```

#### 3 设置文件状态标志

```
flag = flag | O_APPEND;

fcntl(fd, F_SETFL, flag)
```

#### 4 常用的属性标志

```
O_APPEND-----设置文件打开为末尾添加

O_NONBLOCK-----设置打开的文件描述符为非阻塞
```



<br /><br />

```c
//修改文件描述符的flag属性
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>

int main(int argc, char *argv[])
{
	//打开文件
	int fd = open(argv[1], O_RDWR);
	if(fd<0)
	{
		perror("open error");
		return -1;
	}

	//获得和设置fd的flags属性
	int flags = fcntl(fd, F_GETFL, 0);
	flags = flags | O_APPEND;
	fcntl(fd, F_SETFL, flags);

	//写文件
	write(fd, "hello world", strlen("hello world"));

	//关闭文件
	close(fd);

	return 0;
}

```

