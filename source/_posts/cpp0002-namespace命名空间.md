---
title: cpp0002-namespace命名空间
date: 2021-04-15 00:43:28
tags:
categories: c++
doc:
---

# namespace命名空间

假如同一个公司有两个张三，当你叫张三的时候，两个张三都会回头。这就出现了二义性。

为了区分这两个张三，就引进了命名空间。

第一个张三可以叫为张三A

第二个张三可以叫为张三B

```c++
#include<iostream>
//方式1，可以直接使用std内的元素
using namespace std;

//方式2，可以直接使用cout

using std::cout;




int main(int argc, char* argv[])
{

	//方式3，通过命名空间名来引用cout
	std::cout;

	return 0;
}



```

```c++
#include<iostream>

using namespace std;

int main(int argc, char* argv[])
{

	{
		//只能在本复合语句内使用
		using std::cout;
	}

	cout << a;//出错

	return 0;
}

```

