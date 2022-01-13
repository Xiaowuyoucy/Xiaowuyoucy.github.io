---
title: 静态库的制作和使用
date: 2022-01-08 13:17:08
tags:
categories: linux
doc:
---

#### 	命名规则：

```
lib + 库的名字 +.a
例如：libMylib.a
```

### 制作步骤：

#### 1).生成对应的.o文件

```
gcc *.c -c
```

​	

#### 2).将生成的.o文件打包

```
ar rcs 静态库的名字 生成的所有.o文件
ar rcs libMylib.a *.o
```

#### 发布和使用静态库

​	1)发布静态库
​	2)头文件

#### 使用:

```
gcc main.c 库的目录和名字 -o main
gcc main.c ./libMylib.a -o main

gcc main.c -L 库的目录 -l 库的名字(将lib和.a去掉) -o main
gcc main.c -L lib -l Mylib -o main
```

### 静态库的优缺点：

#### 优点：

​		发布程序的时候不需要提供对应的库
​		加载库的速度快
​		

#### 缺点；

​	库被打包到应用程序中，导致库的体积很大
​	库发生改变，需要重新编译程序
​	
链接器是以`.o`为单位链接的

#### nm 

```
nm 库的名字/可执行程序
```

可以查看库的一些信息
T 代表在代码区