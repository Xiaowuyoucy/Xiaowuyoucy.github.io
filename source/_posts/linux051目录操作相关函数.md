---
title: 目录操作相关函数
date: 2022-01-10 16:43:59
tags:
categories: linux
doc:
---



#### opendir函数

函数描述:打开一个目录 

函数原型: 

```
DIR *opendir(const char *name);
```

函数返回值: 指向目录的指针

函数参数: 要遍历的目录(相对路径或者绝对路径)

#### readdir函数

函数描述: 读取目录内容--目录项

函数原型: 

```
struct dirent *readdir(DIR *dirp);
```

函数返回值: 读取的目录项指针，有错误发生或读取到目录文件尾则返回NULL。，出错会设置errno变量

成功时，返回1。在目录末尾，返回0。在出现错误时，返回-1，并正确设置errno。

函数参数: opendir函数的返回值

```
struct dirent

{

	 ino_t d_ino;       // 此目录进入点的inode

	 off_t d_off;        // 目录文件开头至此目录进入点的位移

	 signed short int d_reclen;  // d_name 的长度, 不包含NULL 字符

   	unsigned char d_type;   // d_name 所指的文件类型 

 	char d_name[256];	  // 文件名

};
```

 

```
d_type的取值: 
 DT_BLK - 块设备

 DT_CHR - 字符设备
 
 DT_DIR - 目录

 DT_LNK - 软连接

 DT_FIFO - 管道

 DT_REG - 普通文件

 DT_SOCK - 套接字
 
 DT_UNKNOWN - 未知
```

<br/>

![img](/images/javawz/wps8867.tmp.jpg) 



#### closedir函数

 函数描述: 关闭目录

函数原型: 

```
int closedir(DIR *dirp);
```

 函数返回值: 成功返回0, 失败返回-1

 函数参数: opendir函数的返回值

读取目录内容的一般步骤

1 DIR *pDir = opendir(“dir”);  //打开目录

2 while((p=readdir(pDir))!=NULL){} //循环读取文件

3 closedir(pDir);  //关闭目录