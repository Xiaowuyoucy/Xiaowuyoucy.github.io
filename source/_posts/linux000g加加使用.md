---
title: g++使用
date: 2022-02-10 01:01:38
tags:
categories: linux
doc:
---

g\++是GNU组织推出的C\++编译器。它不但可以用来编译传统的C\++程序，也可以用来编译现代C\++，比如`C++11/14`等。

g\++的用法和gcc类似，编译C\++的时候比gcc更简单，因为它会自动链接到C\++标准库，而不像gcc需要手工指定。

g\++编译程序的内部过程和gcc一样，也要经过4个阶段：预处理、编译、汇编和链接。

g\++的基本语法格式如下：

```
g++ [选项] 准备编译的文件 [选项] [目标文件]
```

### 编译单个源文件

```
g++ test.cpp -o test
```

test.cpp为需要编译的源文件，test为输出的可执行目录

### 编译多个源文件

```
g++ testspeaker.cpp speaker.cpp -o testspeaker
```

testspeaker.cpp speaker.cpp 需要编译的2个源文件

### 更多参数详解

**-E**
 只激活预处理,这个不生成文件,你需要把它重定向到一个输出文件里面.

```bash
g++ -E test.cpp > test.txt
```

**-S**
 只激活预处理和编译，就是指把文件编译成为汇编代码。

```bash
g++ -S test.cpp
```

他将生成.s的汇编代码，你可以用文本编辑器察看

**-c**
 只激活预处理,编译,和汇编,也就是他只把程序做成obj文件

```bash
gcc -c hello.c
```

他将生成.o的obj文件

### 目录选项

`-Wl:rpath`,添加运行时库路径
`-Wl:rpath`, 后面也是路径，运行的时候用。这条编译指令会在编译时记录到target文件中，所以编译之后的target文件在执行时会按这里给出的路径去找库文件。

如：`-Wl:rpath=/home/hello/lib`

表示将`/home/hello/lib`目录作为程序运行时第一个寻找库文件的目录，程序寻找顺序是：`/home/hello/lib–>/usr/lib–>/usr/local/lib。`

可以加多个包含路径，程序在运行时的寻找顺序为添加的顺序。

`-L`,添加链接库路径
`-L `后跟路径，告诉链接器从哪找库(.so文件)，只有在链接时会用到。

如：`-L /home/hello/lib`

表示将`/home/hello/lib`目录作为第一个寻找库文件的目录，寻找顺序是：`/home/hello/lib–>/usr/lib–>/usr/local/lib`。

可以加多个包含路径，链接器的寻找顺序为添加的顺序。

`-l`,添加引用链接库
`-l` 在链接时用到，它的作用是告诉链接器，要用到哪个库。 如：`-l pthread`

告诉链接器(linker)，程序需要链接pthread这个库,这里的pthread是库名不是文件名，具体来说文件句是libpthread.so

`-I`,添加包含路径
`-I `在编译时用，告诉编译器去哪个路径下找文件

如：`-I /home/hello/include`

表示将`/home/hello/include`目录作为第一个寻找头文件的目录。

编译器的寻找顺序是：`/home/hello/include–>/usr/include–>/usr/local/include`。如果在`/home/hello/include`中有个文件hello.h，则在程序中用#include就能引用到这个文件。

可以加多个包含路径，编译器的寻找顺序为添加的顺序。

### 调试选项

`-g`
只是编译器，在编译的时候，产生调试信息。

`-gstabs`
此选项以stabs格式声称调试信息,但是不包括gdb调试信息.

`-gstabs+`
此选项以stabs格式声称调试信息,并且包含仅供gdb使用的额外调试信息.

`-ggdb`
此选项将尽可能的生成gdb的可以使用的调试信息.

`-glevel`
请求生成调试信息，同时用level指出需要多少信息，默认的level值是2
链接选项

`-static `此选项将禁止使用动态库。
优点：程序运行不依赖于其他库

缺点：文件比较大

`-shared` 指定生成动态链接库
优点：生成文件比较小

缺点：运行时需要系统提供动态库

`-symbolic` 建立共享目标文件的时候,把引用绑定到全局符号上.
对所有无法解析的引用作出警告(除非用连接编辑选项 `-Xlinker -z -Xlinker defs`取代)。

注：只有部分系统支持该选项.

### 错误与警告

`-Wall`
一般使用该选项，允许发出GCC能够提供的所有有用的警告。也可以用-W{warning}来标记指定的警告。

`-pedantic`
允许发出ANSI/ISO C标准所列出的所有警告

`-pedantic-errors`
允许发出ANSI/ISO C标准所列出的错误

`-werror`
把所有警告转换为错误，以在警告发生时中止编译过程

`-w`
关闭所有警告,建议不要使用此项
预处理选项

`-Dmacro`
相当于C语言中的#define macro

`-Dmacro=defn`
相当于C语言中的#define macro=defn

`-Umacro`
相当于C语言中的#undef macro

`-undef`
取消对任何非标准宏的定义
其他选项

`-o`
制定目标名称,缺省的时候,g++编译出来的文件是a.out

```
-O
-O0
-O1
-O2
-O3
```

编译器的优化选项的4个级别，\-O0表示没有优化,\-O1为缺省值，-O3优化级别最高

`-fpic`
编译器就生成位置无关目标码.适用于共享库(shared library).

`-fPIC`
编译器就输出位置无关目标码.适用于动态连接(dynamic linking),即使分支需要大范围转移.

`-v`
显示详细的编译、汇编、连接命令