---
title: lseek函数
date: 2022-01-10 00:25:39
tags:
categories: linux
doc:
---

要包含头文件:

```
#include <sys/types.h>
#include <unistd.h>
```



函数原型:

```
off_t lseek(int fd, off_t offset, int whence);

参数1 文件描述符
参数2 参数offset 为根据参数whence 来移动读写位置的位移数。
参数3 whence 为下列其中一种:
        SEEK_SET 参数offset 即为新的读写位置.
        SEEK_CUR 以目前的读写位置往后增加offset 个位移量.
        SEEK_END 将读写位置指向文件尾后再增加offset 个位移量. 当whence 值为SEEK_CUR 或
        SEEK_END 时, 参数offet 允许负值的出现.


下列是教特别的使用方式:
1) 欲将读写位置移到文件开头时:lseek(int fildes, 0, SEEK_SET);
2) 欲将读写位置移到文件尾时:lseek(int fildes, 0, SEEK_END);
3) 想要取得目前文件位置时:lseek(int fildes, 0, SEEK_CUR);

```

返回值：当调用成功时则返回目前的读写位置, 也就是距离文件开头多少个字节. 若有错误则返回-1, errno 会存放错误代码.

#### 获取文件大小

```
int size = lseek(fd,0,SEEK_END);
printf("size:%d \n",size);
```



#### 文件拓展:

就是将文件扩大

```
lseek(fd,2000,SEEK_END);	//拓展2000个字节
write(fd,'a',1);	//写一些字符后生效
```

拓展之后需要手动写随便一些东西才生效





```c
#include <sys/types.h>
#include <unistd.h>
#include<fcntl.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<stdio.h>
#include<stdlib.h>

int main(){
	
	int fd1 = open("./newfile",O_RDWR | O_CREAT,0664);
		
	if(fd1 == -1){
		perror("open:");
		exit(-1);
	}
	//获取文件大小
	int size = lseek(fd1,0,SEEK_END);
	printf("size:%d\n",size);
	
	//拓展文件
	int ret = lseek(fd1,2000,SEEK_END);
	printf("%d\n",ret);
	write(fd1,"a",1);
	
	
	close(fd1);
	
	return 0;
}
```

