---
title: 第一个Windows程序
date: 2021-05-30 14:34:55
tags:
categories: windows程序设计2
doc:
---

# 第一个Windows程序





```cpp
#include<iostream>
#include<windows.h>

using namespace std;

//int WINAPI WinMain(HINSTANCE hInstance,HINSTANCE hPrevInstance,PSTR szCmdLine,int iCmdShow)
/*

WINAPI 是____stdcall
WinMain 是入口函数
HINSTANCE实例句柄类型
HINSTANCE hInstance	本模块的实例句柄
HINSTANCE hPrevInstance 是旧时代的,Win16留下来的,
PSTR szCmdLine 命令行参数
int iCmdShow 主窗口初始化时显示方式

*/
int WINAPI WinMain(HINSTANCE hInstance,HINSTANCE hPrevInstance,PSTR szCmdLine,int iCmdShow)
{

	MessageBox(NULL,TEXT("我爱你,小麦老婆"),TEXT("这是标题"),MB_OK);
	return 0;
}

/*
MessageBox(NULL,TEXT("我爱你,小麦老婆"),TEXT("这是标题"),MB_OK);
MessageBox 对话框函数
参数1: 窗口句柄
参数2:对话框内容
参数3:标题
参数4:标志	显示消息框中的按钮以及图标。

*/
```

WINAPI 是__stdcall

WinMain 是入口函数

HINSTANCE实例句柄类型

HINSTANCE hInstance	本模块的实例句柄

HINSTANCE hPrevInstance 是旧时代的,Win16留下来的,

PSTR szCmdLine 命令行参数

int iCmdShow 主窗口初始化时显示方式



MessageBox(NULL,TEXT("我爱你,小麦老婆"),TEXT("这是标题"),MB_OK);

MessageBox 对话框函数

参数1: 窗口句柄

参数2:对话框内容

参数3:标题

参数4:标志	显示消息框中的按钮以及图标。