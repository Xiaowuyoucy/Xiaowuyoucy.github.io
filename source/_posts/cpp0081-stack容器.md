---
title: stack容器
date: 2021-07-05 13:40:14
tags:
categories: c++
doc:
---

# stack容器

Stack简介

stack是堆栈容器，是一种“先进后出”的容器。

stack是简单地装饰deque容器而成为另外的一种容器。

`#include <stack> `

#### stack对象的默认构造

```
stack采用模板类实现， stack对象的默认构造形式： stack <T> stkT;  
stack <int> stkInt;            //一个存放int的stack容器。
stack <float> stkFloat;     //一个存放float的stack容器。
stack <string> stkString;     //一个存放string的stack容器。
...				    
//尖括号内还可以设置指针类型或自定义类型。

```

### stack的push()与pop()方法  

```
stack.push(elem);   //往栈头添加元素
stack.pop();   //从栈头移除第一个元素
```

```
stack<int> stkInt;  	
stkInt.push(1);stkInt.push(3);stkInt.pop();   
stkInt.push(5);stkInt.push(7);  
stkInt.push(9);stkInt.pop();   	
stkInt.pop();  
此时stkInt存放的元素是1,5  
```

#### stack对象的拷贝构造与赋值

```
stack(const stack &stk);		     //拷贝构造函数
stack& operator=(const stack &stk);	//重载等号操作符
```

```
stack<int> stkIntA;
stkIntA.push(1);
stkIntA.push(3);
stkIntA.push(5);
stkIntA.push(7);
stkIntA.push(9);

stack<int> stkIntB(stkIntA);		//拷贝构造
stack<int> stkIntC;
stkIntC = stkIntA;				//赋值

```

### stack的数据存取

```
stack.top();	  //返回最后一个压入栈元素
```



```


stack<int> stkIntA;
		stkIntA.push(1);
		stkIntA.push(3);
		stkIntA.push(5);
		stkIntA.push(7);
		stkIntA.push(9);

		int iTop = stkIntA.top();		//9
		stkIntA.top() = 19;			//19

```

### stack的大小

```
stack.empty();   //判断堆栈是否为空
stack.size(); 	     //返回堆栈的大小
```

```
stack<int> stkIntA;
		stkIntA.push(1);
		stkIntA.push(3);
		stkIntA.push(5);
		stkIntA.push(7);
		stkIntA.push(9);

		if (!stkIntA.empty())
		{
			int iSize = stkIntA.size();		//5
		}

```

