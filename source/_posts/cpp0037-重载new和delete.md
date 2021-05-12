---
title: 重载new和delete
date: 2021-05-11 23:07:33
tags:
categories: c++
doc:
---

# 重载new和delete

### 声明语法

```cpp
	void * operator new(size_t size);
	void * operator new[](size_t size);

	void operator delete[](void *p);
	void operator delete(void * p);
```



```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>


using namespace std;

class A
{
public:
	A();
	A(int t) { test = t; }
	~A();

	/*
	重载了new 和 delete后,一样会调用构造函数和析构函数
	*/

	void * operator new(size_t size);
	void * operator new[](size_t size);

	void operator delete[](void *p);
	void operator delete(void * p);
private:
	size_t _size;
	int test;
};





void A::operator delete[](void * p)
{
	free(p);
}

void * A::operator new[](size_t size)
{
	return malloc(size);
}

void A::operator delete(void * p)
{
	free(p);
}
void * A::operator new(size_t size)
{
	cout << size << endl;
	return malloc(size);
}





A::A()
{
	cout << "A()..." << endl;
}

A::~A()
{
	cout << "~A()..." << endl;
}

int main(char *argv[], int argc)
{
	A * a = new A();//a.operator new(sizeof(A))
	//初始化一个new A数组
	A * b = new A[10]{(10),(20),(30)};
	delete[] b;
	delete a;

	return 0;
}

```

