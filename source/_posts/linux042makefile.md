---
title: makefile
date: 2022-01-08 13:37:34
tags:
categories: linux
doc:
---

项目代码管理工具

#### 1).命名规则

Makefile或makefile

#### 2).makefile的规则:

规则中有三要素:目标,依赖,命令

```
make 
make命令运行makefile
```

![image-20211228222234708](/images/javawz/image-20211228222234708-1640701376106.png)

```
目标:依赖
	命令	//命令左边一定要有一个tab缩进


app:main.c add.c sub.c mul.c
	gcc main.c add.c sub.c mul.c -o app
```

![image-20211229002343337](/images/javawz/image-20211229002343337-1640708638230.png)





![image-20211229002843041](/images/javawz/image-20211229002843041.png)

```
main:main.o add.o		
	gcc main.o add.o -o main

main.o:main.c
	gcc -c main.c

add.o:add.c
	gcc -c add.c
```

当add.c的修改时间 大于add.o的修改时间时,表示文件过时了

终极目标一定要在最前面

<br/><br/>

### makefile中的变量

```
变量名=值
$(变量名) 代表取变量的值
```

```
obj=main.o add.o
target=main
$(target):$(obj)
	gcc $(obj) -o $(target)

%.o:%.c
	gcc -c $< -o $@

```

```
%.o:%.c代表自动匹配,当main.o找不到的时候,会自动匹配%.o:%.c,然后变成main.o:main.c
```

<br/>

#### makefile中的自动变量

```
$< 规则中的第一个依赖
$@ 规则中的目标
$^ 规则中的所有依赖
只能在规则的命令中使用
```

<br/>

#### 由Makefile维护的一些变量

```
通常都是大写
CC:默认值cc
CPPFLAGS:预处理器需要的选项 -I
CFLAGS:编译的时候使用的参数 -Wall -g -c
LDFLAGS: 链接库使用的选项 -L -l
```



```
obj=main.o add.o
target=main
CC=gcc

$(target):$(obj)
	$(CC) $(obj) -o $(target)

%.o:%.c
	$(CC) -c $< -o $@


```

<br/><br/>

### makefile中的函数

所有的函数都有返回值

####  wildcard函数

取目录所有参数指定的文件

```
$(wildcard 参数)
$代表取函数值
src=$(wildcard ./*.c)
获取当前目录的所有.c文件
```

#### patsubst函数

替换函数

```
$(patsubst 被替换的字符串, 要替换的字符串,源字符串)
obj=$(patsubst ./%.c, ./%.o,$(src))
将变量src中所有.c字符串替换成.o字符串
```



```
  1 #obj=main.o add.o
  2 src=$(wildcard ./*.c)
  3 obj=$(patsubst ./%.c, ./%.o,$(src))
  4 
  5 target=main
  6 CC=gcc
  7 CFLAGS=-g
  8 $(target):$(obj)
  9         $(CC) $(obj) -o $(target) $(CFLAGS)
 10 
 11 %.o:%.c
 12         $(CC) -c $< -o $@
 13 


```

```
clean:
	rm $(obj) $(target)
删除所有变量obj和target文件
 使用
 make clean
```



#### 伪目标

```
.PHONY:目标名
```

不进行更新操作.

例如当前目录下有一个clean文件,当执行make clean之后会显示当前已是最新,加上伪目标之后会忽略更新，直接运行

```
.PHONY:clean
clean:
	rm $(obj) $(target)
```



#### 忽略命令错误

在命令前面加上`-`表示如果当前命令出错了，自动忽略错误，继续执行下一条命令

```
clean:
	-rm $(obj) $(target)
```



<br/>

<br/><br/><br/><br/>



