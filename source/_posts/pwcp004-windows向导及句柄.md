---
title: windows向导及句柄
date: 2021-05-31 10:11:55
tags:
categories: windows程序设计2
doc:
---

# windows向导及句柄

- <a href="#1">最好把`stdafx.h`中的#define WIN32_LEAN_AND_MEAN 注释掉,不然很多功能会使用不了 </a>

- <a href="#2">当水平或垂直方向发生变化时就重画 </a>

- <a href="#3">窗口在创建过程中触发的第一个消息 </a>

- <a href="#4">PlaySound 用于播放音乐的API函数 </a>

- <a href="#5">RECT矩形类 </a>

- <a href="#6">HDC设备上下文句柄 </a>

- <a href="#7">PAINTSTRUCT 包含了某应用程序用来绘制它所拥有的窗口客户区所需要的信息的结构体 </a>

- <a href="#8">BeginPaint 指定窗口进行绘图工作的准备，并用将和绘图有关的信息填充到一个PAINTSTRUCT结构中 </a>

- <a href="#9">EndPaint释放设备上下文句柄 </a>

- <a href="#10">GetClientRect获取窗口客户区大小 </a>

- <a href="#11">Ellipse在窗口客户区画一个椭圆 </a>

- <a href="#12">DrawText	在窗口打印文字 </a>

- <a href="#13">句柄 </a>

  

### windows向导

<br>

![1622427535392](/images/javawz/1622427535392.png)

![1622427560513](/images/javawz/1622427560513.png)

<span id="1"></span>

### 最好把`stdafx.h`中的#define WIN32_LEAN_AND_MEAN 注释掉,不然很多功能会使用不了

```c
//#define WIN32_LEAN_AND_MEAN             // 从 Windows 头中排除极少使用的资料
```

<span id="2"></span>

### 当水平或垂直方向发生变化时就重画

```c
wcex.style = CS_HREDRAW | CS_VREDRAW;//当水平或垂直方向发生变化时就重画
```

<span id="3"></span>

### 窗口在创建过程中触发的第一个消息

```
case WM_CREATE:
```

<span id="4"></span>

### PlaySound 用于播放音乐的API函数

```c
BOOL PlaySound(LPCSTR pszSound, HMODULE hmod,DWORD fdwSound);
```

 在vs2010以上版本需要加入#pragma comment(lib, "winmm.lib")才能使用PlaySound 或在项目属性->链接器->输入->附加依赖项中添加winmm.lib

SND_ASYNC 异步播放 

SND_FILENAME 文件名

```c
PlaySound(TEXT("2.wav"),NULL,SND_FILENAME | SND_ASYNC); 
```

<span id="5"></span>

### RECT矩形类

```c
typedef struct _RECT {
LONG left;
LONG top;
LONG right;
LONG bottom;
} RECT, *PRECT;
```

<span id="6"></span>

### HDC设备上下文句柄

```c
HDC hdc; //设备上下文句柄
```

<span id="7"></span>

### PAINTSTRUCT 包含了某应用程序用来绘制它所拥有的窗口客户区所需要的信息的结构体

```c
typedef struct tagPAINTSTRUCT {
HDC hdc;
BOOL fErase;
RECT rcPaint;
BOOL fRestore;
BOOL fIncUpdate;
BYTE rgbReserved[32];
} PAINTSTRUCT, *PPAINTSTRUCT;
```

```c
PAINTSTRUCT ps; //绘制结构
```

<span id="8"></span>

### BeginPaint 指定窗口进行绘图工作的准备，并用将和绘图有关的信息填充到一个PAINTSTRUCT结构中

```c
HDC BeginPaint(
HWND hwnd, // 窗口的句柄
LPPAINTSTRUCT lpPaint // 绘制信息
);
```

```c
hdc = BeginPaint(hwnd,&ps); //获取设备上下文句柄
```

<span id="9"></span>

### EndPaint释放设备上下文句柄

```c
BOOL EndPaint(
HWND hWnd, // 窗口句柄
CONST PAINTSTRUCT *lpPaint // 绘制窗口的数据
);
```

```
EndPaint(hwnd,&ps);		//释放设备上下文句柄
```

<span id="10"></span>>

### GetClientRect获取窗口客户区大小

```c
BOOL GetClientRect(
HWND hWnd, // 窗口句柄
LPRECT lpRect // 客户区坐标
);
```

```c
GetClientRect(hwnd, &rect);	//获取窗口客户区大小
```

<span id="11"></span>

### Ellipse在窗口客户区画一个椭圆

```c
BOOL Ellipse(HDC hdc,	//设备环境句柄。
int nLeftRect, 			//指定限定矩形左上角的X坐标。
int nTopRect, 			//指定限定矩形左上角的Y坐标。
int nRightRect, 		//指定限定矩形右下角的X坐标。
int nBottomRect 		//指定限定矩形右下角的Y坐标。
);
```

```
Ellipse(hdc, 0, 0, 200, 100); //在窗口客户区画一个椭圆
```

<span id="12"></span>

### DrawText	在窗口打印文字

```c
int DrawText(
HDC hDC, // 设备描述表句柄
LPCTSTR lpString, // 将要绘制的字符串
int nCount, // 字符串的长度
LPRECT lpRect, // 指向矩形结构RECT的指针
UINT uFormat // 正文的绘制选项
);
```

```c
DrawText(hdc, TEXT("Hello Windows SDK"), -1, &rect,
	DT_SINGLELINE | DT_CENTER | DT_VCENTER
);
```

DT_SINGLELINE&emsp;&emsp;	 单行显示文本，回车和换行符都不断行。 

DT_CENTER&emsp;&emsp;			 指定文本水平居中显示。 

DT_VCENTER&emsp;&emsp;			 指定文本垂直居中显示。该标记只在单行文本输出时有效，所以它必须与DT_SINGLELINE结合使用。 

<br>

<span id="13"></span>

### 句柄

|           |                |
| :-------: | :------------: |
| HINSTANCE |   实例化句柄   |
|   HWND    |    窗口句柄    |
|    HDC    | 设备上下文句柄 |
|   HICON   |    图标句柄    |
|  HCURSOR  |    鼠标句柄    |
|  HBRUSH   |    画刷句柄    |

<br><br><br>

```c
// windows向导.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "windows向导.h"

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
    LoadStringW(hInstance, IDC_WINDOWS, szWindowClass, MAX_LOADSTRING);
    MyRegisterClass(hInstance);

    // 执行应用程序初始化: 
    if (!InitInstance (hInstance, nCmdShow))
    {
        return FALSE;
    }

    HACCEL hAccelTable = LoadAccelerators(hInstance, MAKEINTRESOURCE(IDC_WINDOWS));

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

    wcex.style          = CS_HREDRAW | CS_VREDRAW;//当水平或垂直方向发生变化时就重画
    wcex.lpfnWndProc    = WndProc;
    wcex.cbClsExtra     = 0;
    wcex.cbWndExtra     = 0;
    wcex.hInstance      = hInstance;
    wcex.hIcon          = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_WINDOWS));
    wcex.hCursor        = LoadCursor(nullptr, IDC_ARROW);
    wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
    //wcex.lpszMenuName   = MAKEINTRESOURCEW(IDC_WINDOWS);
	wcex.lpszMenuName = NULL;
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
	RECT rect;


    switch (message)
    {
    //case WM_COMMAND:
    //    {
    //        int wmId = LOWORD(wParam);
    //        // 分析菜单选择: 
    //        switch (wmId)
    //        {
    //        case IDM_ABOUT:
    //            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
    //            break;
    //        case IDM_EXIT:
    //            DestroyWindow(hWnd);
    //            break;
    //        default:
    //            return DefWindowProc(hWnd, message, wParam, lParam);
    //        }
    //    }
    //    break;


	case WM_CREATE: //窗口在创建过程中触发的第一个消息
		//SND_ASYNC 异步播放 
		//SND_FILENAME 文件名
		//需要在项目属性->链接器->输入->附加依赖项中添加winmm.lib
		//PlaySound(TEXT("2.wav"),NULL,SND_FILENAME | SND_ASYNC); 

		break;
    case WM_PAINT:
	{
		PAINTSTRUCT ps;
		HDC hdc = BeginPaint(hWnd, &ps);
		// TODO: 在此处添加使用 hdc 的任何绘图代码...
		GetClientRect(hWnd, &rect);
		Ellipse(hdc,0,0,rect.right,rect.bottom);
		DrawText(hdc, TEXT("我爱你"), -1, &rect, DT_SINGLELINE | DT_CENTER | DT_VCENTER);


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

