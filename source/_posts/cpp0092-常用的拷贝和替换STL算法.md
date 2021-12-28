---
title: 常用的拷贝和替换STL算法
date: 2021-07-26 22:23:54
tags:
categories: cpp
doc:
---

# 常用的拷贝和替换STL算法



#### copy()

将容器1中的元素拷贝到容器2中

`copy(容器1.begin(),容器1.end(),容器2.begin())`

```
vector<int> vecIntA;

​          vecIntA.push_back(1);

​          vecIntA.push_back(3);

​          vecIntA.push_back(5);

​          vecIntA.push_back(7);

​          vecIntA.push_back(9);

 

​          vector<int> vecIntB;

​          vecIntB.resize(5);            //扩大空间

 

​          copy(vecIntA.begin(), vecIntA.end(), vecIntB.begin()); //vecIntB: {1,3,5,7,9}
```



#### replace() 

`replace(beg,end,oldValue,newValue)`:

  将指定范围内的所有等于oldValue的元素替换成newValue。

​     

```
     vector<int> vecIntA;

​          vecIntA.push_back(1);

​          vecIntA.push_back(3);

​          vecIntA.push_back(5);

​          vecIntA.push_back(3);

​          vecIntA.push_back(9);

 

​          replace(vecIntA.begin(), vecIntA.end(), 3, 8);     //{1,8,5,8,9}
```



#### replace_if()

`replace_if(begin,end,一元谓词,要替换的数字)`

replace_if : 将指定范围内所有操作结果为true的元素用新值替换。

用法举例：

replace_if(vecIntA.begin(),vecIntA.end(),GreaterThree,newVal)

其中 vecIntA是用vector\<int>声明的容器

GreaterThree 函数的原型是 bool GreaterThree(int iNum)

 

```
//把大于等于3的元素替换成8

​          vector<int> vecIntA;

​          vecIntA.push_back(1);

​          vecIntA.push_back(3);

​          vecIntA.push_back(5);

​          vecIntA.push_back(3);

​          vecIntA.push_back(9);

 

​          replace_if(vecIntA.begin(), vecIntA.end(), GreaterThree, 8);     // GreaterThree的定义在上面。
```



#### swap() 

swap:  交换两个容器的元素

   `swap(容器1,容器2)`

```
       vector<int> vecIntA;

​          vecIntA.push_back(1);

​          vecIntA.push_back(3);

​          vecIntA.push_back(5);

​          

​          vector<int> vecIntB;

​          vecIntB.push_back(2);

​          vecIntB.push_back(4);

 

​          swap(vecIntA, vecIntB); //交换
```



<hr/>

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<vector>
#include<algorithm>


using namespace std;

void printV(int &x) {
	cout << x << " " ;
}

void test_copy() {
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(21);
	v1.push_back(31);
	v1.push_back(41);
	v1.push_back(51);

	vector<int> v2;
	v2.resize(v1.size());
	copy(v1.begin(), v1.end(),v2.begin());

	for_each(v2.begin(),v2.end(),printV);
	cout << endl;


}

void test_replace() {
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(1);
	v1.push_back(1);
	v1.push_back(21);
	v1.push_back(31);
	v1.push_back(41);
	v1.push_back(51);

	//将v1的所以元素是1的,都替换成5
	replace(v1.begin(),v1.end(),1,5);
	for_each(v1.begin(),v1.end(),printV);
	cout << endl;
}

bool MyGreate(int & i) {
	if (i >= 1) {
		return true;
	}
	return false;
}

void test_replaceif() {
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(1);
	v1.push_back(1);
	v1.push_back(21);
	v1.push_back(51);

	//将v1容器中大于等于1的所有元素都替换成5
	replace_if(v1.begin(),v1.end(), MyGreate,5);
	for_each(v1.begin(),v1.end(),printV);
	cout << endl;
}


void test_swap() {
	vector<int> v1, v2;
	v1.push_back(1);
	v1.push_back(3);
	v1.push_back(5);

	v2.push_back(2);
	v2.push_back(4);
	v2.push_back(6);

	//交换前
	cout << "v1: ";
	for_each(v1.begin(), v1.end(), printV);
	cout << endl;


	swap(v1, v2);


	//交换后
	cout << "v1: ";
	for_each(v1.begin(),v1.end(),printV);
	cout << endl;

}

int main(char *argv[], int argc)
{
	test_copy();
	test_replace();
	test_replaceif();
	test_swap();
	return 0;
}

```

