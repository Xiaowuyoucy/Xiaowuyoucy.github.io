---
title: 画直线
date: 2021-06-07 17:32:12
tags:
categories: windows程序设计2
doc:
---

#  画直线

### POINT坐标结构类型

```
	typedef struct tagPOINT
	{
		LONG  x;
		LONG  y;
	} POINT, *PPOINT, NEAR *NPPOINT, FAR *LPPOINT;
```



### MoveToEx设置当前画笔位置

```
WINGDIAPI BOOL WINAPI MoveToEx(
HDC hdc,
int X,
int Y,
LPPOINT lpPoint
);
```

HDC hdc：传入参数，[设备上下文](https://baike.baidu.com/item/设备上下文)句柄。

int X：传入参数：新位置的X坐标。

int Y：传入参数：新位置的Y坐标。

LPPOINT lpPoint：传出参数：一个指向POINT结构的[指针](https://baike.baidu.com/item/指针)，用来存放上一个点的位置，若此参数为NULL，则不保存上一个点的位置

返回值：

返回TRUE代表移动成功，FALSE代表失败，用GetLastError获得更具体的错误信息

```
MoveToEx(hdc, 20, 20, NULL);
```



### LineTo从画笔当前位置画到终点位置

```
WINGDIAPI BOOL WINAPI LineTo(HDChdc,intX,intY,);
```

hdc:设备场景句柄

X:线段终点X坐标位置，采用逻辑坐标表示。这个点不会实际画出来；它不属于线段的一部份

Y:线段终点Y坐标位置，采用逻辑坐标表示。这个点不会实际画出来；它不属于线段的一部份

返回值：

返回TRUE代表移动成功，FALSE代表失败

```
//从起点画到200, 20,并改变当前画笔位置
LineTo(hdc, 200, 20);
```



### Polyline

从apt数组中获取坐标点,画一个矩形,不改变当前画笔位置

```
BOOL Polyline( HDChdc, CONST POINT*lppt, intcPoints)
```

hdc ------------ Long，要在其中绘图的设备场景

lpPoint -------- POINTAPI，nCount POINTAPI结构[数组](https://baike.baidu.com/item/数组)中的第一个POINTAPI结构

nCount --------- Long，lpPoint数组中的点数。会从第一个点到第二个点画一条线，以次类推

返回值: bool，非零表示成功，零表示失败 

```
	POINT apt[5] = {
		{600,30},
		{800,30},
		{800,500},
		{600,500},
		{600,30}
	};
//从apt数组中获取坐标点,画一个矩形,不改变当前画笔位置
Polyline(hdc,apt,5);
```



### PolylineTo

从apt2数组中获取坐标点,画一个矩形,并改变当前画笔位置

使用目前位置作为开始点，并将目前位置设定为最后一根线的终点,根据apt的点依次画直线。设置目前位置可调用MoveToEx函数.

```
BOOL PolyLineTo(HDC hdc, CONST POINT * apt, DWORD cpt);
```

参数：

hdc:设备场景句柄

apt:nCount POINTAPI结构[数组](https://baike.baidu.com/item/数组)中的第一个POINTAPI结构

cpt:Point数组中的点数

```
POINT apt2[5] = {
	{ 900,30 },
	{ 1200,30 },
	{ 1200,500 },
	{ 900,500 },
	{ 900,30 }
};

//设置当前画笔位置
MoveToEx(hdc, apt2[0].x, apt2[0].y, NULL);
//从apt2数组中获取坐标点,画一个矩形,改变当前画笔位置
PolylineTo(hdc, apt2 + 1, 4);

```



<hr>

```c
// 直线.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "直线.h"

#define MAX_LOADSTRING 100

// 全局变量: 
HINSTANCE hInst;                                // 当前实例
WCHAR szTitle[MAX_LOADSTRING];                  // 标题栏文本
WCHAR szWindowClass[MAX_LOADSTRING];            // 主窗口类名

// 此代码模块中包含的函数的前向声明: 
ATOM                MyRegisterClass(HINSTANCE hInstance);
BOOL                InitInstance(HINSTANCE, int);
LRESULT CALLBACK    WndProc(HWND, UINT, WPARAM, LPARAM);
INT_PTR CALLBACK    About(HWND, UINT, WPARAM, LPARAM);

int APIENTRY wWinMain(_In_ HINSTANCE hInstance,
                     _In_opt_ HINSTANCE hPrevInstance,
                     _In_ LPWSTR    lpCmdLine,
                     _In_ int       nCmdShow)
{
    UNREFERENCED_PARAMETER(hPrevInstance);
    UNREFERENCED_PARAMETER(lpCmdLine);

    // TODO: 在此放置代码。

    // 初始化全局字符串
    LoadStringW(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
    LoadStringW(hInstance, IDC_MY, szWindowClass, MAX_LOADSTRING);
    MyRegisterClass(hInstance);

    // 执行应用程序初始化: 
    if (!InitInstance (hInstance, nCmdShow))
    {
        return FALSE;
    }

    HACCEL hAccelTable = LoadAccelerators(hInstance, MAKEINTRESOURCE(IDC_MY));

    MSG msg;

    // 主消息循环: 
    while (GetMessage(&msg, nullptr, 0, 0))
    {
        if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }
    }

    return (int) msg.wParam;
}



//
//  函数: MyRegisterClass()
//
//  目的: 注册窗口类。
//
ATOM MyRegisterClass(HINSTANCE hInstance)
{
    WNDCLASSEXW wcex;

    wcex.cbSize = sizeof(WNDCLASSEX);

    wcex.style          = CS_HREDRAW | CS_VREDRAW;
    wcex.lpfnWndProc    = WndProc;
    wcex.cbClsExtra     = 0;
    wcex.cbWndExtra     = 0;
    wcex.hInstance      = hInstance;
    wcex.hIcon          = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_MY));
    wcex.hCursor        = LoadCursor(nullptr, IDC_ARROW);
    wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
    wcex.lpszMenuName   = MAKEINTRESOURCEW(IDC_MY);
    wcex.lpszClassName  = szWindowClass;
    wcex.hIconSm        = LoadIcon(wcex.hInstance, MAKEINTRESOURCE(IDI_SMALL));

    return RegisterClassExW(&wcex);
}

//
//   函数: InitInstance(HINSTANCE, int)
//
//   目的: 保存实例句柄并创建主窗口
//
//   注释: 
//
//        在此函数中，我们在全局变量中保存实例句柄并
//        创建和显示主程序窗口。
//
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // 将实例句柄存储在全局变量中

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}

//
//  函数: WndProc(HWND, UINT, WPARAM, LPARAM)
//
//  目的:    处理主窗口的消息。
//
//  WM_COMMAND  - 处理应用程序菜单
//  WM_PAINT    - 绘制主窗口
//  WM_DESTROY  - 发送退出消息并返回
//
//
LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
	//POINT 坐标结构类型
	/*
	typedef struct tagPOINT
	{
		LONG  x;
		LONG  y;
	} POINT, *PPOINT, NEAR *NPPOINT, FAR *LPPOINT;
	*/

	//第二个矩形
	POINT apt[5] = {
		{600,30},
		{800,30},
		{800,500},
		{600,500},
		{600,30}
	};
	//第三个矩形
	POINT apt2[5] = {
		{ 900,30 },
		{ 1200,30 },
		{ 1200,500 },
		{ 900,500 },
		{ 900,30 }
	};

    switch (message)
    {
    case WM_COMMAND:
        {
            int wmId = LOWORD(wParam);
            // 分析菜单选择: 
            switch (wmId)
            {
            case IDM_ABOUT:
                DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
                break;
            case IDM_EXIT:
                DestroyWindow(hWnd);
                break;
            default:
                return DefWindowProc(hWnd, message, wParam, lParam);
            }
        }
        break;
    case WM_PAINT:
        {
            PAINTSTRUCT ps;
            HDC hdc = BeginPaint(hWnd, &ps);
            // TODO: 在此处添加使用 hdc 的任何绘图代码...

			//设置起点位置
			MoveToEx(hdc, 20, 20, NULL);
			//从起点画到200, 20,并改变当前画笔位置
			LineTo(hdc, 200, 20);
			//从当前画笔位置,画到200, 500
			LineTo(hdc, 200, 500);
			LineTo(hdc, 20, 500);
			LineTo(hdc, 20, 20);

			//从apt数组中获取坐标点,画一个矩形,不改变当前画笔位置
			Polyline(hdc,apt,5);


			//设置当前画笔位置
			MoveToEx(hdc, apt2[0].x, apt2[0].y, NULL);
			//从apt2数组中获取坐标点,画一个矩形,改变当前画笔位置
			PolylineTo(hdc, apt2 + 1, 4);

            EndPaint(hWnd, &ps);


        }
        break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// “关于”框的消息处理程序。
INT_PTR CALLBACK About(HWND hDlg, UINT message, WPARAM wParam, LPARAM lParam)
{
    UNREFERENCED_PARAMETER(lParam);
    switch (message)
    {
    case WM_INITDIALOG:
        return (INT_PTR)TRUE;

    case WM_COMMAND:
        if (LOWORD(wParam) == IDOK || LOWORD(wParam) == IDCANCEL)
        {
            EndDialog(hDlg, LOWORD(wParam));
            return (INT_PTR)TRUE;
        }
        break;
    }
    return (INT_PTR)FALSE;
}

```

