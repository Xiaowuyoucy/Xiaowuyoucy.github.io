---
title: Queue容器
date: 2021-07-05 13:48:08
tags:
categories: cpp
doc:
---

# Queue容器

 queue是队列容器，是一种“先进先出”的容器。

queue是简单地装饰deque容器而成为另外的一种容器。

#include \<queue> 

#### queue对象的默认构造

```
queue采用模板类实现，queue对象的默认构造形式：queue<T> queT;  如：
queue<int> queInt;            //一个存放int的queue容器。
queue<float> queFloat;     //一个存放float的queue容器。
queue<string> queString;     //一个存放string的queue容器。
...				    
//尖括号内还可以设置指针类型或自定义类型。

```

#### queue的push()与pop()方法

```
queue.push(elem);   //往队尾添加元素
queue.pop();   //从队头移除第一个元素
```

```
queue<int> queInt;
queInt.push(1);queInt.push(3);
queInt.push(5);queInt.push(7);
queInt.push(9);queInt.pop();
queInt.pop();
此时queInt存放的元素是5,7,9

```

#### queue对象的拷贝构造与赋值

```
queue(const queue &que);		     //拷贝构造函数
queue& operator=(const queue &que);	//重载等号操作符
```

```
queue<int> queIntA;
		queIntA.push(1);
		queIntA.push(3);
		queIntA.push(5);
		queIntA.push(7);
		queIntA.push(9);

		queue<int> queIntB(queIntA);	//拷贝构造
		queue<int> queIntC;
		queIntC = queIntA;				//赋值

```

#### queue的数据存取

```
queue.back();   //返回最后一个元素
queue.front();   //返回第一个元素
```

```
queue<int> queIntA;
		queIntA.push(1);
		queIntA.push(3);
		queIntA.push(5);
		queIntA.push(7);
		queIntA.push(9);

		int iFront = queIntA.front();		//1
		int iBack = queIntA.back();		//9

		queIntA.front() = 11;			//11
		queIntA.back() = 19;			//19

```

#### queue的大小

```
queue.empty();   //判断队列是否为空
queue.size(); 	     //返回队列的大小
```

```
	queue<int> queIntA; 	
		queIntA.push(1);   	
		queIntA.push(3);  		
		queIntA.push(5);		
		queIntA.push(7);		
		queIntA.push(9);		

		if (!queIntA.empty())
		{
			int iSize = queIntA.size();		//5
		}

```

