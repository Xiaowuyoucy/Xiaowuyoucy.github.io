---
title: Windows模型
date: 2021-05-30 20:06:53
tags:
categories: windows程序设计2
doc:
---

![1622376428133](/images/javawz/1622376428133.png)

HWND H代表句柄,WND代表窗口

HINSTANCE  实例句柄,只要是一个已经存在的程序或窗口等,都称为实例



```c
#include<iostream>
#include<windows.h>

using namespace std;

//窗口回调函数
LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam);


int WINAPI WinMain(
	HINSTANCE hInstance,
	HINSTANCE hPrevInstance,
	PSTR szCmdLine,
	int iCmdShow
	)
{
	HWND hwnd;		//实例句柄
	MSG msg;		//消息
	WNDCLASS wc;	//窗口类

	//1.设计一个窗口类
	wc.style = 0;										//窗口样式
	wc.lpfnWndProc = (WNDPROC)WndProc;					//窗口回调函数
	wc.cbClsExtra = 0;									//额外窗口类的内存
	wc.cbWndExtra = 0;									//额外窗口内存
	wc.hInstance = hInstance;							//窗口的实例句柄
	wc.hIcon = LoadIcon(NULL,IDI_WINLOGO);				//窗口图标
	wc.hCursor = LoadCursor(NULL, IDC_ARROW);			//箭头鼠标
	wc.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);		//窗口颜色
	wc.lpszMenuName = NULL;								//窗口菜单
	wc.lpszClassName = TEXT("MyWndClass");			//窗口类名


	//2.注册窗口类
	RegisterClass(&wc);

	//3.创建窗口
	hwnd = CreateWindow(
		TEXT("MyWndClass"),			//窗口类名
		TEXT("窗口标题"),			//窗口标题
		WS_OVERLAPPEDWINDOW,		//窗口风格
		CW_USEDEFAULT,				//窗口X坐标位置  默认
		CW_USEDEFAULT,				//窗口Y坐标位置  默认
		CW_USEDEFAULT,				//宽度  默认
		CW_USEDEFAULT,				//高度  默认
		NULL,						//父窗口句柄
		NULL,						//菜单句柄
		hInstance,					//窗口实例句柄
		NULL						//用户数据
		);

	//4.显示和更新窗口
	ShowWindow(hwnd, iCmdShow);
	UpdateWindow(hwnd);

	//5.消息循环
	while (GetMessage(&msg, NULL, 0, 0))
	{
		//把键盘输入翻译成为可传递的消息
		TranslateMessage(&msg);
		//转发消息到窗口过程函数
		DispatchMessage(&msg);
		

	}

	//返回消息参数
	return msg.wParam;
}

LRESULT CALLBACK WndProc(
	HWND hwnd,
	UINT message,
	WPARAM wParam,
	LPARAM lParam
	)
{
	//是一种包含了某应用程序用来绘制它所拥有的窗口客户区所需要的信息的结构体
	PAINTSTRUCT ps; //绘制结构

	HDC hdc; //设备上下文句柄

	RECT rect;	//矩形框

	switch (message)
	{

	case WM_SIZE:		//窗口的大小发生改变时

		return 0;
	case WM_LBUTTONDOWN:
	/*	MessageBox(hwnd, TEXT("Hello Windows SDK"), TEXT("Windows"), MB_OK);*/
		return 0;
	case WM_PAINT:		//打印消息
		hdc = BeginPaint(hwnd,&ps); //获取设备上下文句柄

		GetClientRect(hwnd, &rect);	//获取窗口客户区大小
		Ellipse(hdc, 0, 0, 200, 100); //在窗口客户区画一个椭圆

		//在指定的矩形里写入格式化的正文，根据指定的方法对正文格式化
		//DrawText(hdc, 将要绘制的字符串,字符串的长度,指向矩形结构RECT的指针, 正文的绘制选项)
		DrawText(hdc, TEXT("Hello Windows SDK"), -1, &rect,
			DT_SINGLELINE | DT_CENTER | DT_VCENTER
			);

		EndPaint(hwnd,&ps);		//释放设备上下文句柄
		return 0;

	case WM_DESTROY:		//销毁窗口消息(关闭)
		PostQuitMessage(0);	//将0放入消息队列中,让GetMessage获取,然后退出消息循环
		return 0;
	}
	//把不需要处理的消息交给操作系统处理
	return DefWindowProc(hwnd,message,wParam,lParam);
}
```

