---
title: 类模板中的static成员
date: 2021-06-19 07:33:17
tags:
categories: c++
doc:
---

# 类模板中的static成员

类模板中的static成员单独属于每一个实例化出来的类

也就是说每个具体化类的static成员都是不一样的,没有关联的

例如有一个模板类A中有一个static成员

类`A<int>`和类`A<double>`都有自己的static成员,他们两个的static成员是不同且没有关联的

```cpp
#define  _CRT_SECURE_NO_WARNINGS 
#include <iostream>

using namespace std;


template <class T>
class A
{
public:
	T value;
	static T a;
};

template <class T>
T A<T>::a = 0;//类中的静态成员需要在类的外部进行初始化。

int main(void)
{
	//1 模板类通过二次编译根据调用的代码生成了两个不同的类A  一个是A<int> 一个A<char>
	A<int> a1, a2, a3;
	A<char> b1, b2, b3;


	A<int>::a = 20; //改变A<int>的静态成员
	A<char>::a = 'X';//改变A<char>的静态成员

	cout << "a1:a = " << a1.a << endl; //20
	cout << "b1:a = " << b1.a << endl;//'X'

	cout << "a2:a = " << a2.a << endl;
	cout << "a3:a = " << a3.a << endl;


	cout << "b2:a= " << b2.a << endl;//'X'
	cout << "b3:a = " << b3.a << endl;//'X'


	
	return 0;
}
```

