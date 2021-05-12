---
title: 继承中的static
date: 2021-05-13 01:15:14
tags:
categories: c++
doc:
---

# 继承中的static

![1620839824421](/images/javawz/1620839824421.png)

![1620839885212](/images/javawz/1620839885212.png)

![1620839891459](/images/javawz/1620839891459.png)

![1620840002589](/images/javawz/1620840002589.png)

```cpp
#define  _CRT_SECURE_NO_WARNINGS 
#include <iostream>

using namespace std;

class A
{
public:
	static int s;

private:	

};

int A::s = 0;//静态成员变量要在类的外部初始化

class B :public A
{
public:
private:
};

int main(void)
{
	B b;
	cout << b.s << endl;
	b.s = 100;
	cout << b.s << endl;
	
	cout << A::s << endl;
	
	return 0;
}
```

