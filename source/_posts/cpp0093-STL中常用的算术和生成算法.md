---
title: STL中常用的算术和生成算法
date: 2021-07-26 22:44:33
tags:
categories: c++
doc:
---

# STL中常用的算术和生成算法

#### accumulate()

 accumulate: 对指定范围内的元素求和，然后结果再加上一个由val指定的初始值。

要加上头文件 `#include<numeric>`

```


​          vector<int> vecIntA;

​          vecIntA.push_back(1);

​          vecIntA.push_back(3);

​          vecIntA.push_back(5);

​          vecIntA.push_back(7);

​          vecIntA.push_back(9);

​          int iSum = accumulate(vecIntA.begin(), vecIntA.end(), 100);     //iSum==125
```



#### fill() 

 fill:  将输入值赋给标志范围内的所有元素。

​          

```
vector<int> vecIntA;

​          vecIntA.push_back(1);

​          vecIntA.push_back(3);

​          vecIntA.push_back(5);

​          vecIntA.push_back(7);

​          vecIntA.push_back(9);
		//将vecintA容器中的所有元素都填充为8
​          fill(vecIntA.begin(), vecIntA.end(), 8);       //8, 8, 8, 8, 8
```





<hr/>

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<vector>
#include<algorithm>
#include<numeric>

using namespace std;

void test_accumulate()
{
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(2);
	v1.push_back(3);
	v1.push_back(4);

	//accumulate : 将容器中所有元素相加,然后在加上最后一个参数的值,并返回
	int sum = accumulate(v1.begin(),v1.end(),100);

	cout << sum << endl;

}

void test_fill() {
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(1);
	v1.push_back(1);
	v1.push_back(1);

	//将v1所有元素都填充为100
	fill(v1.begin(),v1.end(),100);

	for (vector<int>::iterator it = v1.begin(); it != v1.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}

int main(char *argv[], int argc)
{
	test_accumulate();
	test_fill();
	return 0;
}

```

