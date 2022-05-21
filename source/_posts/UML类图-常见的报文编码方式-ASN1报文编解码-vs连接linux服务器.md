---
title: UML类图_常见的报文编码方式_ASN1报文编解码_vs连接linux服务器
date: 2022-05-21 11:32:04
tags:
categories: cpp
doc:
---



## 两台机器通信

### 1 两台机器的操作系统平台不同:

  	一台是32为windows, 一台是linux 64系统

不同的平台对数据的处理都可能不一样,比如在windows中的int类型是4个字节,在linux中可能是2个字节。

###   2 网络传输的时候用的是大端模式

  	只要是传输的数据的数据类型超过了一个字节都需要进行网络字节序的转换
  	关于字节序序的函数:

```
htons htonl ntohs ntohl 
h: host			 to: 到		n: network		l: long			s: short
```

​	  假如在从A传递数据给B, 需要传递一个结构体

###   3 字节序对齐不一样

```
     struct test
     {
     	int a;
     	char c;
     	double d;
     }
```

###   4 两台服务的开发语言不同

  	对于C语言, 一个字符占用一个字节
  	对于java, 一个字符占用2个字节
  	
 报文: 其实就是数据.
上述几种情况, 涉及到网络数据传输, 都需要对数据进行编解码操作.





# UML类图:

## 1. 面向对象设计

面向对象设计主要就是使用UML的类图，类图用于描述系统中所包含的类以及它们之间的相互关系，帮助人

们简化对系统的理解，它是**系统分析和设计阶段的重要产物，也是系统编码和测试的重要模型依据**



## 类的UML画法

**类（Class）封装了数据和行为，是面向对象的重要组成部分，它是具有相同属性、操作、关系的对象集合的**

**总称**。在系统中，每个类都具有一定的职责，职责指的是类要完成什么样子的功能，要承担什么样子的义

务。一个类可以有多种职责，但是设计得好的类一般只有一种职责。

假如我现在定义了这么一个类：

```
class Persion 
{
public: 
	string getName()
	{ 
		return name; 
	}

	void setName(string name) { 
		this->name = name;
	} 
	
protected: 
	void playBasketball() { 
		pass();
	}

private: 
	void pass() {

	} 

private: 
	string name = "Jack"; 

};
```





那么此类对应的UML为：

![image-20220522003152929](/images/javawz/image-20220522003152929.png)

看到该图分为三层：最顶层的为类名，中间层的为属性，最底层的为方法。
属性的表示方式为：【可见性】【属性名称】：【类型】={缺省值，可选}
方法的表示方式为：【可见性】【方法名称】（【参数列表】）：【类型】
可见性都是一样的，"-"表示private、"+"表示public、"#"表示protected。







## 3. 继承关系

继承也叫作泛化（Generalization），用于描述父子类之间的关系，父类又称为基类或者超类，子类又称作
派生类。在UML中，泛化关系用带空心三角形的实线来表示。

### 3.1 普通继承关系

假如现在我又定义了一个Student和一个Teacher：

```
class Student : public Persion 
{

public: 
	void study() {} 

private: 
	string studentNo; 

};

class Teacher : public Persion 
{

public: 
	void teach() {} 

private: 
	string teacherNo; 
};
```

那么，用UML表示这种关系应当是：

![image-20220522003744558](/images/javawz/image-20220522003744558.png)







### 3.2 抽象继承关系

> 上面的继承是普通的继承，在C++中，除了普通的继承之外，众所周知的还有一种抽象的继承关系，因此就
> 再讲讲抽象继承关系，作为上面的继承的补充。
>
> 比方说我想实现一个链表（Link），插入（insert）与删除（remove）动作我想让子类去实现，链表本身只
> 实现统计链表中元素个数的动作（count），然后有一个子类单向链表（OneWayLink）去实现父类没有实现
> 的动作，C++代码为：







```
// 抽象类(含有纯虚函数的类)
class Link {

public: 
	virtual void insert() = 0; 

	virtual void remove() = 0;

	int count() {
    		return 0;
        } 

};


// 子类 
class OneWayLink : public Link 
{

public: 

	void insert() {

	}

	void remove() {

	}

};
```

其UML的画法为：

![image-20220522004231464](/images/javawz/image-20220522004231464.png)



> 在UML中，抽象类无论类名还是抽象方法名，都以斜体的方式表示，因为这也是一种继承关系，所以子类与父类通过带空心三角形的实线来联系。





## 4. 关联关系

> 关联（Assocition）关系是类与类之间最常见的一种关系，它是一种结构化的关系，表示一类对象与另一类
> 对象之间有联系，如汽车和轮胎、师傅和徒弟、班级和学生等。在UML类图中，用实线连接有关联关系的对
> 象所对应的类，在C++中通常将一个类的对象作为另一个类的成员变量。关联关系分单向关联、双向关联、
> 自关联，逐一看一下。



### 4.1 单向关联关系

单向关联指的是关联只有一个方向，比如顾客（Customer）拥有地址（Address），其代码实现为:

```
// 地址类 
class Address {

};

// 顾客类 
class Customer 
{
private:
	Address address; 	// 作为成员变量 
};
```

UML的画法为：

![image-20220522004610246](/images/javawz/image-20220522004610246.png)







### 4.2 双向关联关系

>默认情况下的关联都是双向的，比如顾客（Customer）购买商品（Product），反之，卖出去的商品总是与
>某个顾客与之相关联，这就是双向关联。c++ 类的写法为：



```
// 商品类 
class Product {
private: 
	Customer customer; // 该商品属于哪一位顾客, 作为成员变量 
};

// 顾客类 
class Customer {
private:
	Product product[64]; // 给顾客购买了哪些商品, 作为成员变量 
};
```

对应的UML类图应当是：

![image-20220522004827876](/images/javawz/image-20220522004827876.png)





### 4.3 自关联关系

> 自关联，指的就是对象中的属性为对象本身，这在链表中非常常见，单向链表Node中会维护一个它的前驱
> Node，双向链表Node中会维护一个它的前驱Node和一个它的后继Node。就以单向链表为例，它的C++写
> 法为：

```
// 链表节点 
class Node 
{
private: 
	Node* nextNode; // 指向后继节点的指针, 作为成员变量 
};
```

对应的UML类图应当是：

![image-20220522004959775](/images/javawz/image-20220522004959775.png)

## 5. 聚合关系

> 聚合（Aggregation）关系表示整体与部分的关系。在聚合关系中，成员对象是整体的一部分，但是成员对象
> 可以脱离整体对象独立存在。在UML中，
>
> 聚合关系用带空心菱形的直线表示，如汽车（Car）与引擎（Engine）、轮胎（Wheel）、车灯（Light），C++ 表示为：



```
class Engine {};

class Wheel {};

class Light {};

class Car {

public: 

	Car(Engine engine, Light light, Wheel wheel) 
	{ 
		this->engine = engine; 
		this->light = light;
    	this->wheel = wheel; 
    }

	void drive() {} 

private: 

	Engine engine; 

	Light light;

	Wheel wheel; 

};
```

对应的UML类图为：

![image-20220522005440449](/images/javawz/image-20220522005440449.png)



> 代码实现聚合关系，**成员对象通常以构造方法、Setter方法的方式注入到整体对象之中**。



## 6. 组合关系

> 组合（Composition）关系也表示的是一种整体和部分的关系，但是在组合关系中整体对象可以控制成员对
> 象的生命周期，一旦整体对象不存在，成员对象也不存在，整体对象和成员对象之间具有同生共死的关系。 在UML中组合关系用带实心菱形的直线表示。
> 比如人的头（Head）和嘴巴（Mouth）、鼻子（Nose），嘴巴和鼻子是头的组成部分之一，一旦头没了，
> 嘴巴也没了，因此头和嘴巴、鼻子是组合关系，C++ 表示为：

```
class Mouth {

};

class Nose {

};


class Head {
public: 
	Head() 
	{ 
		mouth = new Mouth(); 
		nose = new Nose(); 	
	}
	void shake() {
	
	} 

private: 
	Mouth *mouth;
	Nose *nose;
};
```

其UML的表示方法为：

![image-20220522005903188](/images/javawz/image-20220522005903188.png)



>代码实现组合关系，通常在整体类的构造方法中直接实例化成员类，因为组合关系的整体和部分是共生关
>系，如果通过外部注入，那么即使整体不存在，那么部分还是存在的，这就相当于变成了一种聚合关系了。



## 7. 依赖关系

> 依赖（Dependency）关系是一种使用关系，特定事物的改变有可能会影响到使用该事物的其他事物，在需
>要表示一个事物使用另一个事物时使用依赖关系，大多数情况下依赖关系体现在某个类的方法使用另一个类
>的对象作为参数。在UML中，依赖关系用带箭头的虚线表示，由依赖的一方指向被依赖的一方。
>
>比如，驾驶员（Driver）开车，Driver类的drive()方法将车（Car）的对象作为一个参数传递，以便在drive()
>方法中能够调用car的move()方法，且驾驶员的drive()方法依赖车的move()方法，因此也可以说Driver依赖
>Car，C++代码为：



```
class Car 
{
public: 
	void move(); 
};


class Driver {
public: 
	void drive(Car car) 
	{ 
		car.move(); 
	}

};
```

其UML的画法为：

![image-20220522010251666](/images/javawz/image-20220522010251666.png)

> 依赖关系通常通过三种方式来实现：
>
> 1. 将一个类的对象作为另一个类中方法的参数
> 2. 在一个类的方法中将另一个类的对象作为其对象的局部变量
> 3. 在一个类的方法中调用另一个类的静态方法



## 8. 关联关系、聚合关系、组合关系之间的区别

> 从上文可以看出，关联关系、聚合关系和组合关系三者之间比较相似，本文的最后就来总结一下这三者之间
> 的区别。
>
> 关联和聚合的区别主要在于语义上：关联的两个对象之间一般是平等的，聚合则一般是不平等的。
>
> 聚合和组合的区别则在语义和实现上都有差别：组合的两个对象之间生命周期有很大的关联，被组合的对象
> 在组合对象创建的同时或者创建之后创建，在组合对象销毁之前销毁，一般来说被组合对象不能脱离组合对
> 象独立存在，而且也只能属于一个组合对象；聚合则不一样，被聚合的对象可以属于多个聚合对象。
>
> 再举例子来说：
>
> - 你和你的朋友属于关联关系，因为你和你的朋友之间的关系是平等的，关联关系只是表示一下两个对象
>   之间的一种简单的联系而已，就像我有一个朋友
>
> - 你和你借的书属于聚合关系，第一是因为书可以独立存在，第二是因为书不仅仅属于你，也可以属于别
>   人，只是暂时你拥有
>
> - 你和你的心脏属于组合关系，因为你的心脏只是属于你的，不能脱离与你而存在
>
> 
>
> 不过，实际应用中，我个人感觉三种关系其实没有区分得这么清楚，有些架构师甚至会说"组合和聚合没什么
> 区别"，所以，有时候不需要把细节扣得这么细，合理利用对象之间的关系给出设计方案即可。





## 常用的几种序列化方式

1. XML类似于HTML，与HTML很相似，但是用它来序列化对象的时候，就显得很复杂
2. JSON使用起来很简单，他的产生来自于一种关联数组，其本质是采用“键值对”的方式描述对象
3. Protocol Buffer是一个高效的数据化数据存储格式，用于结构化数据串行化，很适合做数据储存或RPC数据交换格式
4. ASN.1抽象语法标记
5. boost 序列化的类
6. 自定义的格式



### ASN.1

头文件和源代码下载地址:https://gitee.com/xiaochenyan/mypro/tree/master/%E5%A4%9A%E7%AB%AF%E5%AE%89%E5%85%A8%E5%8D%8F%E8%AE%AE%E4%BC%A0%E8%BE%93%E5%B9%B3%E5%8F%B0/%E5%8F%82%E8%80%83%E4%BB%A3%E7%A0%81



描述了对一种数据进行表示、编码、传输和解码的数据格式。

- ASN.1它有两部分
  1. 一部分描述信息内数据，数据类型及序列格式；
     - 相当于属性
  2. 一部分描述如何将各个部分组成消息
     - 相当于方法



![在这里插入图片描述](/images/javawz/c3d36423a938455a9551fc5b00ff0542.png)



### 编码格式（TLV）

| TAG（type） | LENGTH   | VALUE    |
| ----------- | -------- | -------- |
| 数据类型    | 数据长度 | 数据的值 |



```c
typedef struct _Persion{
	int age;
	char name[64];
}Persion;
```

![image-20220522013950586](/images/javawz/image-20220522013950586.png)



```c
typedef struct ITCAST_ANYBUF_ {

	unsigned char 	  *pData;		// 数据值
	ITCAST_UINT32     dataLen;		// 数据长度	

	ITCAST_UINT32     unusedBits;	/* for bit string */
	ITCAST_UINT32     memoryType;
	ITCAST_UINT32     dataType;		// 数据类型
	struct ITCAST_ANYBUF_ *next;    /* for sequence and set */
	struct ITCAST_ANYBUF_ *prev;

}ITCAST_ANYBUF;
```



```

ITCAST_INT DER_ItAsn1_WriteInteger(ITCAST_UINT32 integer, ITASN1_INTEGER **ppDerInteger);
函数说明:对整形数据进行编码操作
函数参数:
	integer: 输入参数, 表示待编码的整形数据
	ppDerInteger: 传出参数, 编码之后的数据
返回值:
	成功或者失败
	
	
	
ITCAST_INT DER_ItAsn1_ReadInteger(ITASN1_INTEGER *pDerInteger, ITCAST_UINT32 *pInteger);
函数说明: 对整形数据解码
参数说明: 
	pDerInteger: 传入参数, 表示待解码的数据
	pInteger: 传出参数, 表示解码之后的数据
返回值:
	成功或者失败
	  ITCAST_ANYBUF p;
	  ITCAST_UINT32 aa; 
例如: DER_ItAsn1_ReadInteger(&p, &aa);



ITCAST_INT DER_ItAsn1_WritePrintableString(ITASN1_PRINTABLESTRING *pPrintString, ITASN1_PRINTABLESTRING **ppDerPrintString);
函数说明:编码字符串数据
函数参数:
	pPrintString: 输入参数, 表示要编码的数据
	ppDerPrintString: 输出参数, 表示编码之后的数据
返回值:
	成功或者失败	



ITCAST_INT DER_ItAsn1_ReadPrintableString(ITASN1_PRINTABLESTRING *pDerPrintString, ITASN1_PRINTABLESTRING **ppPrintString);
函数说明: 解码函数, 将ANYCAST_ANYBUF类型解码到第二个参数
参数说明:
		pDerPrintString: 输入参数, 表示待解码的数据
		ppPrintString: 输出参数, 存放解码之后的数据
返回值:
	成功或者失败



ITCAST_INT DER_ITCAST_String_To_AnyBuf(ITCAST_ANYBUF **pOriginBuf, unsigned char * strOrigin, int strOriginLen);
函数说明: 将char *---->ITCAST_ANYBUF类型
函数参数:
	pOriginBuf: 传出参数, ITCAST_ANYBUF指针
	strOrigin: 传入参数, 待转换的字符串
	strOriginLen: 传入参数, strOrigin的字符串长度
返回值:
	成功或者失败



int EncodeChar(char *pData, int dataLen, ITCAST_ANYBUF **outBuf);
函数说明: 将char *类型数据进行编码
函数参数:
	pData: 输入参数, 指的是待编码的字符串
	dataLen: 输入参数, 指的是pData的长度
	outBuf: 输出参数, ITCAST_ANYBUF类型的数据, TLV格式
	
int DecodeChar(ITCAST_ANYBUF *inBuf, char **Data, int *pDataLen);



ITCAST_INT DER_ItAsn1_WriteSequence(ITASN1_SEQUENCE *pSequence, ITCAST_ANYBUF **ppDerSequence);
函数说明: 序列化链表, 将链表序列化成字节流数据
函数参数:
	pSequence: 输入参数, 待序列化的数据
	ppDerSequence: 输出参数, 序列化之后的数据



ITCAST_INT DER_ItAsn1_ReadSequence(ITCAST_ANYBUF *pDerSequence, ITASN1_SEQUENCE **ppSequence);
函数说明: 反序列化
参数说明:
	pDerSequence:输入参数, 开始需要将char *--->ITCAST_ANYBUF类型
	ppSequence: 输出参数, 获得链表头节点



ITCAST_INT DER_ITCAST_FreeQueue(ITCAST_ANYBUF *pAnyBuf);
释放内存

```



### 代码示例

```
#include <stdio.h>
#include "teacher.h"
#include <string.h>
#include <stdlib.h>

int main()
{
	Teacher tea;
	memset(&tea, 0x00, sizeof(Teacher));
	strcpy(tea.name, "路飞");
	tea.age = 20;
	tea.p = (char*)malloc(100);
	strcpy(tea.p, "我是要成为海贼王的男人");
	tea.plen = strlen(tea.p);

	// 编码
	char* outData;
	int outlen;
	encodeTeacher(&tea, &outData, &outlen);

	//===============================================

	// 解码
	Teacher* pt;
	decodeTeacher(outData, outlen, &pt);
	printf("name:	%s\n", pt->name);
	printf("age:	%d\n", pt->age);
	printf("p:	%s\n", pt->p);
	printf("plen:	%d\n", pt->plen);

	freeTeacher(&pt);

	system("pause");

	return 0;
}
```



### Teacher.h

```
#ifndef _TEACHER_H
#define _TEACHER_H
typedef struct _Teacher
{
	char name[64]; 
	int age;   
	char *p;
	long plen;
}Teacher;

// 编码结构体
//p: 输入参数, 待编码的结构体
//outData: 输出参数, 保存编码之后的字符串
//outlen: 输出参数, 编码之后的字符串的长度
int encodeTeacher(Teacher* p, char** outData, int* outlen);
// 解码结构体

//inData: 输入参数
//inLen: inData的长度
//p:保存解码之后的数据
int decodeTeacher(char* inData, int inLen, Teacher**p);
// 释放内存函数

//释放内存
void freeTeacher(Teacher** p);

#endif	// _TEACHER_H

```



### Teacher.c

```
#include "teacher.h"
#include "itcast_asn1_der.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

/*
	typedef struct _Teacher
	{
		char name[64];
		int age;
		char *p;
		long plen;
	}Teacher;
*/
int encodeTeacher(Teacher * p, char ** outData, int * outlen)
{
	ITCAST_ANYBUF *head = NULL;
	ITCAST_ANYBUF *temp = NULL;
	ITCAST_ANYBUF *next = NULL;

	//编码name
	//ITCAST_INT DER_ItAsn1_WritePrintableString(ITASN1_PRINTABLESTRING *pPrintString, ITASN1_PRINTABLESTRING **ppDerPrintString);
	//char *---->ITCAST_ANYBUF
	DER_ITCAST_String_To_AnyBuf(&temp, p->name, strlen(p->name)+1);
	DER_ItAsn1_WritePrintableString(temp, &head);
	DER_ITCAST_FreeQueue(temp);
	next = head;

	//编码age
	DER_ItAsn1_WriteInteger(p->age, &next->next);
	next = next->next;

	//编码p
	//int EncodeChar(char *pData, int dataLen, ITCAST_ANYBUF **outBuf);
	EncodeChar(p->p, strlen(p->p)+1, &next->next);
	next = next->next;

	//编码plen
	DER_ItAsn1_WriteInteger(p->plen, &next->next);

	//序列化
	DER_ItAsn1_WriteSequence(head, &temp);

	//输出参数赋值
	*outData = temp->pData;
	*outlen = temp->dataLen;

	//释放内存
	DER_ITCAST_FreeQueue(head);

	return 0;
}

/*
	typedef struct _Teacher
	{
		char name[64];
		int age;
		char *p;
		long plen;
	}Teacher;
*/

int decodeTeacher(char * inData, int inLen, Teacher ** p)
{
	ITCAST_ANYBUF *head = NULL;
	ITCAST_ANYBUF *temp = NULL;
	ITCAST_ANYBUF *next = NULL;

	Teacher *pt = (Teacher *)malloc(sizeof(Teacher));
	if (pt == NULL)
	{
		return -1;
	}

	//将inData反序列化成链表
	//将char *--->ITCAST_ANYBUF类型
	DER_ITCAST_String_To_AnyBuf(&temp, inData, inLen);
	DER_ItAsn1_ReadSequence(temp, &head);
	DER_ITCAST_FreeQueue(temp);
	next = head;

	//解码name
	DER_ItAsn1_ReadPrintableString(next, &temp);
	memcpy(pt->name, temp->pData, temp->dataLen);
	next = next->next;
	DER_ITCAST_FreeQueue(temp);

	//解码age
	DER_ItAsn1_ReadInteger(next, &pt->age);
	next = next->next;

	//解码p
	int len = 0;
	DecodeChar(next, &pt->p, &len);
	next = next->next;

	//解码plen
	DER_ItAsn1_ReadInteger(next, &pt->plen);

	//给输出参数赋值
	*p = pt;

	//释放内存
	DER_ITCAST_FreeQueue(head);

	return 0;
}

void freeTeacher(Teacher ** p)
{
	if ((*p) != NULL)
	{
		if ((*p)->p != NULL)
		{
			free((*p)->p);
		}
		free(*p);
	}
}

```



## vs连接linux服务器

创建项目工程

![image-20220522014650992](/images/javawz/image-20220522014650992.png)



点击菜单栏上的工具---------->选项



![image-20220522014903248](/images/javawz/image-20220522014903248.png)



选择  跨平台------->连接管理器----------->添加



![image-20220522015040222](/images/javawz/image-20220522015040222.png)





![image-20220522015129104](/images/javawz/image-20220522015129104.png)



<br />

![image-20220522015211419](/images/javawz/image-20220522015211419.png)



修改生成目录的路径

![image-20220522040443774](/images/javawz/image-20220522040443774.png)

<br />

<br />

<br />

<br />

<br />

<br />

<br />

























