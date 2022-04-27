---
title: define中的特殊符号
date: 2022-03-27 09:45:50
tags:
categories: cpp
doc:
---

**1. 符号"\\"**
 用于多行定义,每行的最后加上"\\"

```cpp
#define MAX(a,b)  \
  if(a>b)\
  		return a;\
  	else\
  		return b;
```

**2.符号"#“和”##"**
 \#是把参数[字符串](https://so.csdn.net/so/search?q=字符串&spm=1001.2101.3001.7020)化, ##是连接两个参数

```cpp
#include <iostream>
using namesapce std;
#define A(x) x
#define B(x) #x
#define C(x,y) x##y
int main()
{
   cout << A("hello")<<endl;
   cout<<B(hello)<<endl;
   cout<<C("hello","world")<<endl;
}
结果：
hello
hello
helloworld
```

###  #@ 字符化操作符

```
#define ToChar(x) #@x
```

`#@x`只能用于有传入参数的宏定义中，且必须置于宏定义体中的参数名前。作用是将传的单字符参数名转换成字符，以一对单引用括起来其实就是给x加上单引号，结果返回是一个`const char`。 
 举例说：

```
char a = ToChar(1);
     ==> char a='1';
```

做个越界试验

```
char a = ToChar(123);
     ==> char a='3';
```

但是如果你的参数超过四个字符，编译器就给给你报错了！`error C2015: too many characters in constant` ：P