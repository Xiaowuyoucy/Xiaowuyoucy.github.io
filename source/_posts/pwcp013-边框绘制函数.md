---
title: 边框绘制函数
date: 2021-06-07 20:08:10
tags:
categories: windows程序设计2
doc:
---

# 边框绘制函数

###  Rectangle矩形

 使用该函数画一个矩形，可以用当前的画笔画矩形轮廓，用当前画刷进行填充。 

```
BOOL Rectangle(HDC hdc, int nLeftRect, int nTopRect, int nRightRect, int nBottomRect)；
```

hdc：设备环境句柄。

nLeftRect：指定矩形左上角的逻辑X坐标。

nTopRect：指定矩形左上角的逻辑Y坐标。

nRightRect：指定矩形右下角的逻辑X坐标。

nBottomRect：指定矩形右下角的逻辑Y坐标。

返回值：如果[函数调用](https://baike.baidu.com/item/函数调用)成功，返回值非零，否则返回值为0。

Windows NT：若想获得更多错误信息，请调用GetLastError函数。

备注：此函数不使用和改变当前位置。

```
Rectangle(hdc,200,50,600,400);
```



### Ellipse画圆或椭圆

```
BOOL Ellipse(HDC hdc,
int nLeftRect,
int nTopRect,
int nRightRect,
int nBottomRect
)；
```

hdc：设备环境句柄。

nLeftRect：指定限定矩形左上角的X坐标。

nTopRect：指定限定矩形左上角的Y坐标。

nRightRect：指定限定矩形右下角的X坐标。

nBottomRect：指定限定矩形右下角的Y坐标。

如果[函数调用](https://baike.baidu.com/item/函数调用)成功，返回值非零；如果函数调用失败，返回值是0。

Windows NT：若想获得更多[错误信息](https://baike.baidu.com/item/错误信息)，请调用GetLastError函数。

```
Ellipse(hdc, 200, 50, 600, 400);
```



### RoundRect带圆角的矩形

 该函数画一个带圆角的矩形，此矩形由当前画笔画轮廓，由当前画刷填充。 

```
BOOL RoundRect(HDC hdc, int nLeftRect, int nTopRect, int nRightRect, int nBottomRect, int nWidth, int nHeight)；
```

参数：

hdc：设备环境句柄。

nLeftRect：指定矩形左上角的X坐标。

nTopRect：指定矩形左上角的Y坐标。

nRightRect：指定矩形右下角的X坐标。

nbottomRect：指定矩形右下角的Y坐标。

nWidth：指定用来画圆角的椭圆的宽。

nHeight：指定用来画圆角的椭圆的高。

返回值：如果[函数调用](https://baike.baidu.com/item/函数调用)成功，则返回值非空，否则返回值是0。

Windows NT：若想获得更多的错误信息，请调用GetLastError函数。

```
RoundRect(hdc, 700, 50, 1100, 400,100,100);
```



### ARC画圆弧

```
BOOL Arc(
HDC hdc,
int xLeft,
int yTop,
int xRight,
int yBottom,
int XStart,
int YStart,
int XEnd,
int YEnd);
```

*hdc* 绘画的窗口句柄

xLeft和nyTopt指定外接矩形左上角坐标

xRight和yBottom指定外接矩形右下角坐标

xStart和yStart指定圆弧开始坐标

xEnd和nyEnd指定圆弧结束坐标

```
Arc(hdc, 850, 500, 1000, 650, 20, 600, 1200, 400);
```

### Chord画弦

```
BOOL Chord(
  DC: HDC;                                {设备环境句柄}
  int X1, Y1, X2, Y2, X3, Y3, X4, Y4 ; {四个坐标点}
);
```

参数表

X1,Y1 ---------- Long，指定围绕椭圆的一个矩形的左上角位置

X2,Y2 ---------- Long，指定围绕椭圆的一个矩形的右下角位置

X3,Y3 ---------- Long，指定与椭圆相交的一条线的一个点

X4,Y4 ---------- Long，指定与椭圆相交的一条线的另一个点

   

 返回值

Long，非零表示成功，零表示失败。会设置[GetLastError](https://baike.baidu.com/item/GetLastError/4278820)

```
Chord(hdc, 450,500,800,850, 20, 600, 1200, 400);
```





### Pie画圆饼

```
BOOL Pie(HDC hdc, int nLeftRect, int nTopRect, int nRightRect, int nBottomRect, int nXRadial1, int nYRadial1, int nXRadial2, int nYRadial2)；
```

hdc：设备环境句柄。

nLeftRect：指定限定矩形左上角的X坐标。

nTopRect：指定限定矩形左上角的Y坐标。

nRigthRect：指定限定矩形右下角的X坐标。

nBottomRect：指定限定矩形右下角的Y坐标。

nXRadial1：指定第一条半径的端点的X坐标。

nYRadial1：指定第一条半径的端点的Y坐标。

nXRadial2：指定第二条半径的端点的X坐标。

nYRadial2：指定第二条半径的端点的Y坐标。

返回值：如果[函数调用](https://baike.baidu.com/item/函数调用)成功，返回值非零；如果函数调用失败，返回值是0。

Windows：要得到更多的错误信息，调用GetLastError。

```
Pie(hdc,50,500,400,850,500,0,50,650);
```

<hr>

```c
// 边框绘制函数.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "边框绘制函数.h"

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

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW ,
     0, 0, GetSystemMetrics(SM_CXSCREEN), GetSystemMetrics(SM_CYSCREEN), nullptr, nullptr, hInstance, nullptr);

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
			//画矩形
			Rectangle(hdc,200,50,600,400);
			//画椭圆或正圆
			Ellipse(hdc, 200, 50, 600, 400);
			//画带圆角的矩形
			RoundRect(hdc, 700, 50, 1100, 400,100,100);

			//画弦
			Chord(hdc, 450,500,800,850, 20, 600, 1200, 400);
			//画弧
			Arc(hdc, 850, 500, 1000, 650, 20, 600, 1200, 400);
			//画圆饼
			Pie(hdc,50,500,400,850,500,0,50,650);
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

