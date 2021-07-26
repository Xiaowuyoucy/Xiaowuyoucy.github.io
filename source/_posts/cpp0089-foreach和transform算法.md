---
title: foreach和transform算法
date: 2021-07-24 23:28:16
tags:
categories: c++
doc:
---

# foreach和transform算法

#### for_each()

`for_each(迭代器起始位置,迭代器结束位置,函数对象或回调函数)`

 for_each: 用指定函数依次对指定范围内所有元素进行迭代访问。该函数不得修改序列中的元素。

for_each()会返回一个新的函数对象

一般情况下：for_each所使用的函数对象，参数是引用，没有返回值

```
void show(const int &iItem)
{
	cout << iItem;
}
main()
{
	int iArray[] = {0,1,2,3,4};
	vector<int> vecInt(iArray,iArray+sizeof(iArray)/sizeof(iArray[0]));
    for_each(vecInt.begin(), vecInt.end(), show);

//结果打印出0 1 2 3 4
}

```

#### transform()

 transform:  与for_each类似，遍历所有元素，但可对容器的元素进行修改

`transform(迭代器起始位置,迭代器结束位置,迭代器x起始位置,函数对象或回调函数)`

transform所使用的函数对象，参数一般不使用引用，而是还有返回值

```
int increase (int i)  
{  
	return i+1;   
}  

main()
		{
			vector<int> vecIntA;
			vecIntA.push_back(1);
			vecIntA.push_back(3);
			vecIntA.push_back(5);
			vecIntA.push_back(7);
			vecIntA.push_back(9);


			transform(vecIntA.begin(),vecIntA.end(),vecIntA.begin(),increase);		//vecIntA : {2,4,6,8,10}
		}

```

<hr	/>




```
#include <iostream>
using namespace std;

#include "string"
#include <vector>
#include <list>
#include "set"
#include <algorithm>
#include "functional"
#include "iterator"  //输出流迭代器的头文件

void printV(vector<int> &v)
{
	for (vector<int>::iterator it=v.begin(); it!=v.end(); it++)
	{
		cout << *it << " ";
	}
}

void printList(list<int> &v)
{
	for (list<int>::iterator it=v.begin(); it!=v.end(); it++)
	{
		cout << *it << " ";
	}
}

void showElem(int &n)
{
	cout << n << " ";
}

class CMyShow
{
public:
	CMyShow()
	{
		num = 0;
	}
	void operator()(int &n)
	{
		num ++;
		cout << n << " ";
	}
	void printNum()
	{
		cout << "num:" << num << endl;
	}
protected:
private:
	int num;
};

void main41_foreach()
{
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(3);
	v1.push_back(5);

	printV(v1);
	cout << endl;

	/*
	template<class _InIt,
	class _Fn1> inline
		_Fn1 for_each(_InIt _First, _InIt _Last, _Fn1 _Func)
	{	// perform function for each element
		_DEBUG_RANGE(_First, _Last);
		_DEBUG_POINTER(_Func);
		return (_For_each(_Unchecked(_First), _Unchecked(_Last), _Func));
	} */

	//函数对象 回调函数入口地址
	for_each(v1.begin(), v1.end(), showElem);
	cout << endl;

	for_each(v1.begin(), v1.end(), CMyShow());
	cout << endl;

	 CMyShow mya;
	 CMyShow my1 = for_each(v1.begin(), v1.end(),mya); //给my1初始化
	 mya.printNum();  //ma1和my1 是两个不同的对象
	 my1.printNum();

	 my1 = for_each(v1.begin(), v1.end(),mya);  //给my1赋值
	 my1.printNum();
}


int  increase(int i)
{
	return i+100;
}

void main42_transform()
{
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(3);
	v1.push_back(5);

	printV(v1);
	cout << endl;

	//transform 使用回调函数
	transform(v1.begin(), v1.end(), v1.begin(),  increase ); 
	printV(v1);
	cout << endl;

	//transform 使用 预定义的函数对象
	transform(v1.begin(), v1.end(), v1.begin(),  negate<int>() ); 
	printV(v1);
	cout << endl;

	//transform 使用 函数适配器 和函数对象
	list<int> mylist;
	mylist.resize( v1.size() );

	transform(v1.begin(), v1.end(), mylist.begin(),  bind2nd( multiplies<int>(), 10 ) ); 
	printList(mylist);
	cout << endl;

	//transform 也可以把运算结果 直接输出到屏幕
	transform(v1.begin(), v1.end(), ostream_iterator<int>(cout, " " ), negate<int>() );
	cout << endl;
}


//一般情况下：for_each所使用的函数对象，参数是引用，没有返回值
//transform所使用的函数对象，参数一般不使用引用，而是还有返回值
int showElem2(int n)
{
	cout << n << " ";
	return n;
}

void main43_transform_pk_foreach()
{
	vector<int> v1;
	v1.push_back(1);
	v1.push_back(3);
	v1.push_back(5);

	vector<int> v2 = v1;

	for_each(v1.begin(), v1.end(), showElem);


	//transform 对 函数对象的要求
	/*
c:\program files\microsoft visual studio 10.0\vc\include\algorithm(1119): 
	参见对正在编译的函数 模板 实例化
		“_OutIt std::_Transform1<int*,_OutIt,
		void(__cdecl *)(int &)>(_InIt,_InIt,_OutIt,_Fn1,
		std::tr1::true_type)”的引用
1>          with
1>          [
1>              _OutIt=std::_Vector_iterator<std::_Vector_val<int,std::allocator<int>>>,
1>              _InIt=int *,
1>              _Fn1=void (__cdecl *)(int &)
1>          ]
	*/

	/*
	template<class _InIt,
	class _OutIt,
	class _Fn1> inline
		_OutIt _Transform(_InIt _First, _InIt _Last,
		_OutIt _Dest, _Fn1 _Func)
	{	// transform [_First, _Last) with _Func
		for (; _First != _Last; ++_First, ++_Dest)
			*_Dest = _Func(*_First);  //解释了 为什么 要有返回值
		return (_Dest);
	}
	*/
	transform(v2.begin(), v2.end(), v2.begin(), showElem2);
}

void main()
{
	//main41_foreach();
	//main42_transform();
	// main43_transform_pk_foreach();
	cout<<"hello..."<<endl;
	system("pause");
	return ;
}
```

