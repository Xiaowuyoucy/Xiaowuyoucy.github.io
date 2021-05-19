---
title: windows程序设计基础
date: 2021-05-19 01:32:42
tags:
categories: windows程序设计
doc:
---

# windows程序设计基础

### Win32 API 简介

API(Application Programming Interface)  应用程序编程接口

提供了各种各样与windows系统服务有关的函数

SDK编程就是直接调用API函数进行编程

Win32 API 是指编制32应用程序时使用的一组函数、结构、宏定义。

### 应用程序类型

windows支持两种类型的应用程序:

Graphical User Interface（GUI ）图形用户界面

Console User Interface（CUI ）控制台用户界面



### API函数的调用方法

```
///////////////////////////////////////////////////////////////
// 01FirstApp.cpp文件
#include <windows.h>	// 包含MessageBox函数声明的头文件

int main(int argc, char* argv[])
{
	// 调用API函数MessageBox
	int nSelect = ::MessageBox(NULL, "Hello, Windows XP", "Greetings", MB_OKCANCEL);
	if(nSelect == IDOK)
		printf(" 用户选择了“确定”按钮 \n");
	else
		printf(" 用户选择了“取消”按钮 \n");
	return 0;
}

```

调用windows API  需要包含`windows.h`头文件

::MessageBox 在函数前面加上`::`表示是一个全局函数与c++成员函数区分开

### 代码风格

#### 变量的命名

【限定范围的前缀】 + 【数据类型前缀】+【有意义的单词】

`g_szTitle`

常量：用全大写字母

全局变量：`g_`开头

sz开头：表示以`‘\0’`结尾的字符串

类的成员变量：以`m_`开头

局部变量：【数据类型前缀】+【有意义的单词】bResult   b代表BOOL类型

函数名：每个单词首字母大写 `CreateFile`

类名：`CStudent`  C代表class

结构体：`SStudent`  S代表struct

