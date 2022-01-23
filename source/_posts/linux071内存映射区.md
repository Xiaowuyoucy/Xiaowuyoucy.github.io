---
title: 内存映射区
date: 2022-01-19 19:37:10
tags:
categories: linux
doc:
---

### 存储映射区介绍

​	存储映射`I/O (Memory-mapped I/O) `使一个磁盘文件与存储空间中的一个缓冲区相映射。从缓冲区中取数据，就相当于读文件中的相应字节；将数据写入缓冲区，则会将数据写入文件。这样，就可在不使用read和write函数的情况下，使用地址（指针）完成I/O操作。

使用存储映射这种方法，首先应通知内核，将一个指定文件映射到存储区域中。这个映射工作可以通过mmap函数来实现。

<img src="/images/javawz/image-20220119222706887.png" height="140%" width="35%"/>

 

### mmap函数

```
#include <sys/mman.h>
```



#### 函数作用:

建立存储映射区

#### 函数原型

```
 void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
```

#### 函数返回值：

成功：返回创建的映射区首地址；

失败：`MAP_FAILED`宏 并设置errno

#### 参数：	

addr: 	指定映射的起始地址, 通常设为NULL, 由系统指定

length：映射到内存的文件长度

prot：	映射区的保护方式, 最常用的:

读：`PROT_READ`

写：`PROT_WRITE`

读写：`PROT_READ | PROT_WRITE`

flags：	映射区的特性, 可以是

`MAP_SHARED`: 写入映射区的数据会写回文件, 且允许其他映射该文件的进程共享。

`MAP_PRIVATE`: 对映射区的写入操作会产生一个映射区的复制(copy-on-write), 对此区域所做的修改不会写回原文件。

fd：由open返回的文件描述符, 代表要映射的文件。

offset：以文件开始处的偏移量, **必须是4k的整数倍**, 通常为0, 表示从文件头开始映射。

### munmap函数

```
#include <sys/mman.h>
```



#### 函数作用:

释放由mmap函数建立的存储映射区

#### 函数原型:

```
int munmap(void *addr, size_t length);
```

#### 返回值：

成功：返回0

失败：返回-1，设置errno值

#### 函数参数:

addr：调用mmap函数成功返回的映射区首地址

length：映射区大小（mmap函数的第二个参数）

#### mmap注意事项

创建映射区的过程中，隐含着一次对映射文件的读操作，将文件内容读取到映射区

当MAP_SHARED时，要求：映射区的权限应 <=文件打开的权限(出于对映射区的保护)。而MAP_PRIVATE则无所谓，因为mmap中的权限是对内存的限制。

映射区的释放与文件关闭无关，只要映射建立成功，文件可以立即关闭。

特别注意，当映射文件大小为0时，不能创建映射区。所以，用于映射的文件必须要有实际大小；mmap使用时常常会出现总线错误，通常是由于共享文件存储空间大小引起的。

munmap传入的地址一定是mmap的返回地址。坚决杜绝指针++操作。

文件偏移量必须为0或者4K的整数倍

mmap创建映射区出错概率非常高，一定要检查返回值，确保映射区建立成功再进行后续操作。

#### 有关mmap函数的使用总结

第一个参数写成NULL

第二个参数要映射的文件大小 > 0

第三个参数：PROT_READ 、PROT_WRITE

第四个参数：MAP_SHARED 或者 MAP_PRIVATE

第五个参数：打开的文件对应的文件描述符

第六个参数：4k的整数倍

#### mmap函数相关思考题

可以open的时候O_CREAT一个新文件来创建映射区吗?

答:不可以,因为刚刚创建出来的文件大小是0

如果open时O_RDONLY, mmap时PROT参数指定`PROT_READ|PROT_WRITE`会怎样？

答:open的权限应该大于等于mmap

mmap映射完成之后, 文件描述符关闭，对mmap映射有没有影响？

答:没有影响

如果文件偏移量为1000会怎样？

答:必须为4K的整倍数

对mem越界操作会怎样？

答:出错

如果`mem++`，munmap可否成功？

答:出错

mmap什么情况下会调用失败？

答:指定错误参数或内存不足的时候

如果不检测mmap的返回值，会怎样？

答:如果mmap返回的是-1,而又不检测,从而会使用错误的指针

#### mmap应用练习

练习1：使用mmap完成对文件的读写操作

练习:2：使用mmap完成父子进程间通信

图解说明

![img](/images/javawz/wps1FD2.tmp.jpg) 

思路

调用mmap函数创建存储映射区，返回映射区首地址ptr

调用fork函数创建子进程，子进程也拥有了映射区首地址

父子进程可以通过映射区首地址指针ptr完成通信

调用munmap函数释放存储映射区

```c
//使用mmap函数完成父子进程间通信
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/mman.h>

int main()
{
	//使用mmap函数建立共享映射区
	//void *mmap(void *addr, size_t length, int prot, int flags,
    //              int fd, off_t offset);
	int fd = open("./test.log", O_RDWR);
	if(fd<0)
	{
		perror("open error");
		return -1;
	}

	int len = lseek(fd, 0, SEEK_END);

	void * addr = mmap(NULL, len, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
	//void * addr = mmap(NULL, len, PROT_READ|PROT_WRITE, MAP_PRIVATE, fd, 0);
	if(addr==MAP_FAILED)
	{
		perror("mmap error");
		return -1;
	}
	close(fd);

	//创建子进程
	pid_t pid = fork();
	if(pid<0) 
	{
		perror("fork error");
		return -1;
	}
	else if(pid>0)
	{
		memcpy(addr, "hello world", strlen("hello world"));	
		wait(NULL);
	}
	else if(pid==0)
	{
		sleep(1);
		char *p = (char *)addr;
		printf("[%s]", p);
	}

	return 0;
}

```



练习3：使用mmap完成没有血缘关系的进程间通

思路：两个进程都打开相同的文件，然后调用mmap函数建立存储映射区，这样两个进程共享同一个存储映射区。

#### mmap_read

```c
//使用mmap函数完成两个不相干进程间通信
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/mman.h>

int main()
{
	//使用mmap函数建立共享映射区
	//void *mmap(void *addr, size_t length, int prot, int flags,
    //              int fd, off_t offset);
	int fd = open("./test.log", O_RDWR);
	if(fd<0)
	{
		perror("open error");
		return -1;
	}

	int len = lseek(fd, 0, SEEK_END);

	//建立共享映射区
	void * addr = mmap(NULL, len, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
	if(addr==MAP_FAILED)
	{
		perror("mmap error");
		return -1;
	}

	char buf[64];
	memset(buf, 0x00, sizeof(buf));
	memcpy(buf, addr, 10);
	printf("buf=[%s]\n", buf);

	return 0;
}

```

#### mmap_write

```c
//使用mmap函数完成两个不相干进程间通信
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/mman.h>

int main()
{
	//使用mmap函数建立共享映射区
	//void *mmap(void *addr, size_t length, int prot, int flags,
    //              int fd, off_t offset);
	int fd = open("./test.log", O_RDWR);
	if(fd<0)
	{
		perror("open error");
		return -1;
	}

	int len = lseek(fd, 0, SEEK_END);

	//建立共享映射区
	void * addr = mmap(NULL, len, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
	if(addr==MAP_FAILED)
	{
		perror("mmap error");
		return -1;
	}
	
	memcpy(addr, "0123456789", 10);

	return 0;
}

```



 

#### 使用mmap函数建立匿名映射：

必须是有血缘关系之间的进程使用

```
mmap(NULL, 4096, PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANONYMOUS, -1, 0);
```

`MAP_ANONYMOUS`必须和`MAP_SHARED`一起使用，而且fd指定为-1

```c
//使用mmap匿名映射完成父子进程间通信
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <sys/wait.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/mman.h>

int main()
{
	//使用mmap函数建立共享映射区
	//void *mmap(void *addr, size_t length, int prot, int flags,
    //              int fd, off_t offset);
	void * addr = mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_SHARED | MAP_ANONYMOUS, -1, 0);
	if(addr==MAP_FAILED)
	{
		perror("mmap error");
		return -1;
	}

	//创建子进程
	pid_t pid = fork();
	if(pid<0) 
	{
		perror("fork error");
		return -1;
	}
	else if(pid>0)
	{
		memcpy(addr, "hello world", strlen("hello world"));	
		wait(NULL);
	}
	else if(pid==0)
	{
		sleep(1);
		char *p = (char *)addr;
		printf("[%s]", p);
	}

	return 0;
}

```

