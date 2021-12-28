---
title: C++语言对C的增强和bool
date: 2021-04-15 01:53:25
tags:
categories: cpp
doc:
---

# C++语言对C的增强和bool

- 变量定义
  - c语言:早期版本中规定要定义在函数首部
  - c++:在函数内随便一个位置都可以

- c++对定义全局变量的检测能力增强了,c语言在同一个地方可以定义多次重名的全局变量,c++不可以.

```c
#include<stdio.h>

int g_a;
int g_a = 2;

/*

c语言会处理成:
int g_a;
g_a = 2;


*/

int main(int argc, char* argv[])
{



	return 0;
}
```



- `struct`的类型增强
  - C++定义`struct`变量时可以不用加`struct`关键字

```c++
#include<iostream>

using namespace std;

struct student {
	float score;
};

int main(int argc, char* argv[])
{
	//可以不用加struct关键字
	student xiaoming;
	return 0;
}
```

C++不可以没有函数类型,c语言可以没有函数类型,默认是int

C++填写函数参数必须是对应个数,C语言可以不对应

```c
#include<stdio.h>

//c语言可以没有函数类型,默认为int
fun(){

	return 0;
}

fun1(int a) {
	return 0;
}

int main(int argc, char* argv[])
{

	//传入过多的参数,c语言也不会出错
	fun1(1,2,3,4,5,61,2);
	return 0;
}



```

C++引入了`bool`类型来表示`true`和`false`

```c++
#include<iostream>

using namespace std;

int main(int argc, char* argv[])
{

	bool flag = true;
	cout << (bool)flag << endl;
	cout << sizeof(flag) << endl;
	cout << (flag = 100) << endl;
	cout << (flag = 0) << endl;
	return 0;
}

```

