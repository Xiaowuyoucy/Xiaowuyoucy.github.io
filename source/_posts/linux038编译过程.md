---
title: 编译过程
date: 2022-01-08 13:14:49
tags:
categories: linux
doc:
---



```
源文件---->预处理--->编译--->汇编---->链接---->可执行文件

预处理器(.i)--->编译器(.s)---->汇编器(.o)---->链接器

hello.c--->hello.i----->hello.s-----hello.o----a.out
```


​	

#### 预处理器(cpp):头文件展开,宏替换,注释去掉	

```
gcc -E hello.c -o hello.i
```

#### 编译器(gcc):c文件编程汇编文件	

```
gcc -S hello.i -o hello.s
```

#### 汇编器(as):汇编文件变成二进制文件		

```
gcc -c hello.s -o hello.o
```

#### 链接器(ld):将函数库中相应的代码组合到目标文件中		

```
gcc hello.o -o hello
```

#### 直接生成可执行文件：

```
gcc hello.c -o myapp
gcc hello.c 	//生成一个默认的可执行文件 a.out
```

#### 指定头文件目录

```
gcc hello.c -I ./include -o app
gcc hello.c -I./include -o app 		旧版本的gcc -I后面不可以有空格
```

​	`./include`就是头文件的目录

```
#ifdef DEBUG
printf("123")
#endif
```



#### 通过gcc定义一个DEBUG宏

```
gcc hello.c -o app -D DEBUG
```

#### 优化程序(-O大写)

​	

优化等级 0~3，0不优化

```
gcc hello.c -o app —O3
```

#### 输出警告信息（-Wall）

```
gcc hello.c -o app -Wall
```

#### 添加调试信息（-g）

```
gcc hello.c -o app -g
```

<br/><br/><br/><br/><br/>