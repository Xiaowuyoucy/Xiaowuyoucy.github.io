---
title: cpp介绍
date: 2022-02-22 00:57:55
tags:
categories: cpp17从入门到精通
doc:
---

### 函数

每个c++程序都从main函数开始执行

函数的定义格式:

```
返回值类型 函数名(参数列表)
{
	(多个语句构成的)函数体

}
```



### 语句

C++程序的最小完整执行指令都是以分号结尾的语句。

可以将一条语句写在多个行，不管中间有多少空格、回车符、换行符，最后都是以分号作为语句的结束。



### 程序注释

#### 多行注释

以/\*开头，然后以\*/结束

```
/* 
多行注释
*/
```

#### 单行注释

```
// 单行注释
```

块注释不能嵌套,既不能在块注释中再出现/\* 或 \*/ 

### hello world程序

```c++
#include<iostream>

using namespace std;
int main(){
	cout << "hello world";
	return 0;
}
```

### 标准输入输出库和cout

要使用标准输入输出库就要包含头文件`#include<iostream>`

stream是流的意思，io是输入input输出output的缩写。

`#include`指令称为包含预处理指令,意思是用文件iostream的内容来替换掉这个预处理指令

cout对象代表的是标准输出流对象(既代表终端窗口)



### 命名空间

一个c++程序可能会使用其他人写的库,例如有A库和B库,这两个库中的全局变量或函数的名字都有可能相同,为了避免冲突,c++引入了命名空间将这两个库区分开来。

C++自带的标准库中的所有对象、函数等都属于一个叫std的标准命名空间。

```
using namespace std; 	将整个标准名字空间std的名字都引入到程序中,写上这句话就可以直接调用std命名空间中的变量或函数了
```

#### 名字空间限定

```
命名空间::变量/函数
std::cout
std::cout表示这是名字空间std的cout

std::endl
表示std命名空间中的endl,代表换行符
```



#### 用using引入单个名字

```
using std::cout;
cout << "123" << 456";
引入之后,后面就不需要在用名字限定了

输出运算符<<可以连续使用,这是因为cout << "123" 返回的还是cout
```



### 字符串和字符

单引号括起来的表示一个字符

```
'n'  ','  'a'
```

双引号括起来的表示一个字符串

```
"abd"   "123"    "asd"
```

`\n`和endl的区别

`\n`和endl都表示换行符，endl会强制程序的缓冲区里面的数据立即输出

`\t`表示制表符

### 宏定义

```
#define 宏名 值

#define PI 3.1415
程序预处理阶段是,会将程序中所有的PI都用3.1415来替换
```



### 变量

变量是命名的内存块。

在c++中变量也称为对象

每个变量都有一个数据类型,例如:

```
int i;
double r;
double area;
```



### {}初始值

```
int i{2};
double r{2.5};
double area = 0;
```



### 标准输入流对象cin

cin输入流对象,代表键盘对象

```
double r;
cin >> r;	//重键盘输入一个数字到r中
```



### 用户自定义类型

string类型

要使用string类型需要包含头文件

```
#include<string>
```

string类型有一个size()成员函数,可以返回string对象的字符个数

```
string s1 = "abc";
cout << s1.size() << endl;
```

substr(s,e)成员函数返回string对象下标s到e(不包含e)之间的字符构成的一个字符串

```
string s1 = "123456";
cout << s1.substr(1,4) << endl;	//输出123
```

可以用+将两个字符串对象拼接起来

```
string s1 = "abc";
string s2 = "def";
cout << s1 + s2 << endl;	//输出abcdef
```



### 数字表示

用0b开头表示一串二进制表示一个二进制数

```
0b000010111
```

#### 十六进制

用0x表示16进制数

```
0x12 	0xFA	0xFF
```

#### 八进制

用0开头表示8进制数

```
012		044		0777
```

```
std::dec	// 以十进制方式输出
std::hex	// 以十六进制方式输出
std::oct	// 以八进制方式输出
```





![image-20220222020913683](/images/javawz/image-20220222020913683.png)



### 编译c++程序

```
g++ 源文件名 -o 目标程序名

g++ test.cpp -o test
最终生成test程序
```



## 变量和数据类型

变量的定义格式

```
类型名 变量名{};
```

{}方式的初始化称为列表初始化

如果{}里面没有值,对于基本类型的变量,初始值默认为0,有的编译器则会发出警告或报错

### 信息损失则报错

```
int a{1.2};		//1.2会截取小数部分,导致信息损失,所以会报错
```



### auto

用auto定义一个有初始值的变量时,不需要明确指定类型,因为编译器能自动从变量的初始值推断出该变量的数据类型。

```
auto b = true;			// bool
auto ch{'x'};			// char
auto i = 123;			// int
auto d{1.2};			// double
auto z = d + i;			// 从表达式的d+i的值来推断z的数据类型
```



### typeid运算符

可以用typeid运算符查询得到一个数据类型或变量的信息

```
cout << typeid(int) .name() << endl;
cout << typeid(b).name() << endl;
```



### decltype

用decltype(exp)得到一个表达式的值的类型,并用这个类型来定义一个变量。

```
decltype(3 + 4.5) c;
```



### 基本类型

```
wchar_t 宽字符类型,在windows平台上是16位 2字节类型
char16_t 表示UTF-16类型
char32_t 表示UTF-32字符类型
```



### 整型文字常量

字母u或U表示unsigned整型

字母l或L表示long整型

字母ll或LL表示 long long整型

```
18u			// unsigned
18U			// unsigned

022L		// long
18l			// long 

18LL		// long long
0x12uL		// unsigned long 
18ULL		// long long
```



### 浮点型文字常量

默认是double

用f或F表示float

用l或L表示long double

```
3.14
3.14f
3.14F

3.14L
3.14l
```



### 字符串文字常量

L表示 wchar_t

u表示 char16_t

U表示 char32_t

u8前缀表示 UTF-8

```
L'A'		// wchar_t	

u'A'		// char16_t
	
U'A'		// char32_t

u8'A'		// utf-8
u8"abcd"
```



### 原始字符串

不需要处理转义字符

```
R"1234\n"
```



### 格式化输出

流操作符定义在2个头文件中

```
iomanip.h
ios.h		//ios头文件已经被iostream头文件包含,该头文件中的操纵符不带任何参数
```



#### ios.h

```
std::dec;			// 十进制输出
std::hex;			// 十六进制输出
std::oct;			// 八进制输出

std::fixed;			// 以固定精度形式输出
std::scientific;	// 以科学计数法形式输出
std::hexfloat;		// 以十六进制浮点形式输出
std::defaultfloat; 	// 以默认形式输出
```



#### iomanip.h

iomanip的操纵符需要传递一个参数

```
std::setw(n);				//改变输出域的宽度
std::setprecision(n)		//改变浮点数的精度

//改变填空字符,当setw的输出域宽度大于输出值的宽度时,默认的填空字符是空格,可以用setfill(ch)改变这个填空字符
std::setfill(ch)			

```



```
cout << setprecision(2) << 3.1415926 << endl;
cout << setw(10) << 3.1415926 << endl
cout << setw(10) << setfill('-') << 3.1415926 << endl;

输出
3.1
		3.1
----------3.1
```



### 强制类型转换

```
(类型) 值
(int)123;

static_cast<类型>值
static_cast<int>123;
```



### 类型别名

可以用关键字using给一个数据类型起另外的名字,称为**类型别名**

```
using 别名 = 类型;

using  INT = int;
```

![image-20220223002707798](/images/javawz/image-20220223002707798.png)

上面的意思是 如果定义了 USING_COMPILER_A则执行下面这部分，否则则执行另外这部分。



### 枚举

不同的枚举类型的值是不能相互比较或赋值的 。

```
enum class Day{Monday,Tuesday,Wednesday,Thursday,Friday,Saturday,Sunday};

Day d{Day::Tuesday};	//定义一个Day类型的d变量并初始值为Tuesday
```

