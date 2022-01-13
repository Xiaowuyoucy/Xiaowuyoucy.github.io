---
title: gdb调试
date: 2022-01-08 13:36:16
tags:
categories: linux
doc:
---

可执行程序必须包含调试信息`-g`

```
gdb 程序名
gdb main
```

```
l  输出十行源代码

l 文件名:要查看第几行
l main.c 10

l 文件名:函数名
l main.c main

后面继续按回车可以继续显示源代码,直到输入了其他命令

```

#### 设置断点

```
b 行号/函数名
break 行号/函数名

break 22
b 37
b main
break main

```

### 条件断点

```
b 行号 if 条件
b 15 if i==15  //当i等于15时,设置15行断点
```

#### 查看断点信息

```
i b
或
info break

Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x00000000000006cf in main at main.c:6
enb y表示断点开始,n表示断点关闭
main.c:6 代表在main.c文件的第六行断点

```



#### 删除断点

```
需要通过 info break 找到断点编号

d 编号
d 4
```



```
start 执行一步
```

#### 单步调试

```
n
```

#### 继续执行

```
c 
代表执行到断点的位置
```



#### 进入函数内部

```
s
```



#### 查看变量的值

```
p 变量名
p i
```



#### 查看变量的类型

```
ptype 变量名
ptype i
```



#### 追踪变量的值

```
display 变量名
display i

取消追踪变量
undisplay 追踪变量的编号
undisplay 1
查看追踪变量的编号
info display
```



#### 跳出当次循环

```
u
```



#### 跳出当前函数

```
finish
```

循环有断点需要先删除断点



#### 设置变量的值

```
set var 变量名=值
set var i=10

```



#### 退出gdb

```
quit
```





