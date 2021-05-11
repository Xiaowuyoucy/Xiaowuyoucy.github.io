---
title: new和delete操作符
date: 2021-05-10 20:08:40
tags:
categories: c++
doc:
---

# new和delete操作符

在软件开发过程中，常常需要动态地分配和撤销内存空间，例如对动态
链表中结点的插入与删除。在C语言中是利用库函数malloc和free来分配和撤
销内存空间的。C++提供了较简便而功能较强的运算符new和delete来取代
malloc和free函数。

`new和delete是运算符，不是函数，因此执行效率高。`

用new分配数组空间时不能指定初值。如果由于内存不足等原因而无法正
常分配空间，则new会返回一个空指针NULL，用户可以根据该指针的值判断分
配空间是否成功。
`malloc不会调用类的构造函数,而new会调用类的构造函数`
`free不会调用类的析构函数，而delete会调用类的析构函数`

![1620648607782](/images/javawz/1620648607782.png)

```cpp

#include<iostream>


using namespace std;

void test() {
	//使用new开辟一个int类型变量空间
	int * ip = new int;
	//使用new开辟一个int类型数组空间
	int * arrayP = new int[10];

	if (ip != NULL) {

		*ip = 100;
		cout << *ip << endl;
		//释放ip空间
		delete ip;
		ip = NULL;
	}

	if (arrayP != NULL)
	{
		for (int i = 0; i < 10; i++)
		{
			arrayP[i] = i + 1;
			cout << arrayP[i] << " ";
		}
		cout << endl;
		//释放arrayP数组指针
		delete[] arrayP;
		arrayP = NULL;
	}
}

int main(char *argv[], int argc)
{
	test();
	return 0;
}

```



```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
#include<cstring>

using namespace std;

class Student {
public:
	Student(){
		name = NULL;
		cout << "Student()" << endl;
	}

	Student(const char *tname,int txueHao,float tscore) {

		cout << "Student(char *tname,int txueHao,float tscore)" << endl;
		const int len = strlen(tname) + 1;
		name = new char[len];
		strcpy(name, tname);
		xueHao = txueHao;
		score = tscore;
	}

	~Student()
	{
		cout << "~Student()" << endl;
		if (name != NULL)
		{
			delete [] name;
			name = NULL;
		}
	}

private:
	char * name;
	int xueHao;
	float score;

};

void test1() {
	Student *s1 = new Student();//触发无参构造
	Student	*s2 = new Student("XiaoMing", 123, 100.0f);//触发有参构造

	int *ip = new int(10);//new一个int类型存储空间并赋于10;


	delete s1;//触发析构函数
	delete s2;//触发析构函数
	delete ip;

}
int main(char *argv[], int argc)
{
	test1();

	return 0;
}

```

