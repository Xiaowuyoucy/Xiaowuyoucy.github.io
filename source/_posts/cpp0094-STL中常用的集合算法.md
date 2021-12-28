---
title: STL中常用的集合算法
date: 2021-07-26 23:13:19
tags:
categories: cpp
doc:
---

# STL中常用的集合算法

#### set_union(),set_intersection(),set_difference()

 set_union: 构造一个有序序列，包含两个有序序列的并集。

 set_intersection: 构造一个有序序列，包含两个有序序列的交集。



 假设有集合A和B，所有属于A且不属于B的元素的集合被称为A与B的差集。

示例：对于集合A = {a, b, c, d}和集合B = {b, c, w}，则A与B 的差集为{a, d}

也就是集合A中有的元素,在集合B中没有的这部分元素，就是集合A和集合B中的差集



 set_difference: 构造一个有序序列，该序列保留第一个有序序列中存在而第二个有序序列中不存在的元素。

```
vector<int> vecIntA;

​          vecIntA.push_back(1);

​          vecIntA.push_back(3);

​          vecIntA.push_back(5);

​          vecIntA.push_back(7);

​          vecIntA.push_back(9);

 

​          vector<int> vecIntB;

​          vecIntB.push_back(1);

​          vecIntB.push_back(3);

​          vecIntB.push_back(5);

​          vecIntB.push_back(6);

​          vecIntB.push_back(8);

 

​          vector<int> vecIntC;

​          vecIntC.resize(10);

 

​          //并集

​          set_union(vecIntA.begin(), vecIntA.end(), vecIntB.begin(), vecIntB.end(), vecIntC.begin());          //vecIntC : {1,3,5,6,7,8,9,0,0,0}

 

​          //交集

​          fill(vecIntC.begin(),vecIntC.end(),0);

​          set_intersection(vecIntA.begin(), vecIntA.end(), 			 		
vecIntB.begin(), vecIntB.end(), vecIntC.begin());        //vecIntC: {1,3,5,0,0,0,0,0,0,0}

 

​          //差集

​          fill(vecIntC.begin(),vecIntC.end(),0);

​          set_difference(vecIntA.begin(), vecIntA.end(), vecIntB.begin(), 			  
           vecIntB.end(), vecIntC.begin());          //vecIntC: {7,9,0,0,0,0,0,0,0,0}
```





<hr />

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

void printV(int &x) {
	cout << x << " ";
}

void test_union() {
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(3);
	v1.push_back(5);

	vector<int> v2;
	v2.push_back(2);
	v2.push_back(4);
	v2.push_back(6);

	vector<int> v3;
	v3.resize(v1.size() + v2.size());
	//v1∪v2,把相同部分去掉,将结果存放到v3容器中
	set_union(v1.begin(),v1.end(),v2.begin(),v2.end(),v3.begin());

	for_each(v3.begin(),v3.end(), printV);
	cout << endl;
}

void test_set_intersection() {
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(3);
	v1.push_back(5);

	vector<int> v2;
	v2.push_back(1);
	v2.push_back(3);
	v2.push_back(6);

	vector<int> v3;
	v3.resize(v1.size()+v2.size());
	//v1∩v2,将不相同的部分去掉,把结果存放到v3容器中
	set_intersection(v1.begin(),v1.end(),v2.begin(),v2.end(),v3.begin());
	for_each(v3.begin(), v3.end(), printV);
	cout << endl;

}

void test_difference() {
	vector<int> v1;

	v1.push_back(1);
	v1.push_back(2);
	v1.push_back(3);
	v1.push_back(4);
	v1.push_back(4);
	v1.push_back(6);

	vector<int> v2;

	v2.push_back(1);
	v2.push_back(2);
	v2.push_back(3);
	v2.push_back(5);

	vector<int> v3;
	v3.resize(v1.size() + v2.size());

	/*
	  set_difference 差集:	v1 - v2 将剩余元素存放在v3中

      假设有集合A和B，所有属于A且不属于B的元素的集合被称为A与B的差集。
      
      示例：对于集合A = {a, b, c, d}和集合B = {b, c, w}，则A与B 的差集为{a, d}

	*/
	set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());
	for_each(v3.begin(), v3.end(), printV);
	cout << endl;


}

int main(char *argv[], int argc)
{
	test_union();
	test_set_intersection();
	test_difference();

	return 0;
}

```

