---
title: read和write函数
date: 2022-01-08 23:04:52
tags:
categories: linux
doc:
---

### read函数

```
要包含头文件
#include <unistd.h>

函数原型
ssize_t read(int fd, void *buf, size_t count);

参数1 int fd, 		//文件描述符
参数2 void *buf,		//缓冲区地址
参数3 size_t count //要读取的字节个数

返回值
成功返回读取的字节数，出错返回-1并设置errno，如果在调read之前已到达文件末尾，则这次read返回0。

```



### write函数

```
要包含头文件
#include <unistd.h>

函数原型
ssize_t write(int fd, const void *buf, size_t count);

fd：文件描述符；
buf：指定的缓冲区，即指针，指向一段内存单元；
nbyte：要写入文件指定的字节数；

返回值：写入文档的字节数（成功）；-1（出错）
write函数把buf中nbyte写入文件描述符handle所指的文档，成功时返回写的字节数，错误时返回-1.

```

