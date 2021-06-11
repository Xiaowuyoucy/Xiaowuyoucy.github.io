---
title: 画笔
date: 2021-06-08 12:52:49
tags:
categories: windows程序设计2
doc:
---

# 画笔

### HPEN创建画笔句柄变量

```
 HPEN hPen1;
```

### CreatePen创建画笔

```
HPEN CreatePen(int nPenStyle, int nWidth, COLORREF crColor);
```

nPenStyle ------ Long，指定画笔样式，可以是下述常数之一

PS_SOLID

画笔画出的是实线

PS_DASH

画笔画出的是虚线（nWidth必须不大于1）

PS_DOT

画笔画出的是点线（nWidth必须不大于1）

PS_DASHDOT

画笔画出的是点划线（nWidth必须不大于1）

PS_DASHDOTDOT

画笔画出的是点-点-划线（nWidth必须不大于1）

PS_NULL

画笔不能画图

PS_INSIDEFRAME

由椭圆、矩形、圆角矩形、饼图以及弦等生成的封闭对象框时，画线宽度向内扩展。如指定的准确RGB颜色不存在，就进行抖动处理

nWidth --------- Long，以逻辑单位表示的画笔的宽度

crColor -------- Long，画笔的RGB颜色

[返回值]

Long，如函数执行成功，就返回指向新画笔的一个句柄；否则返回零

[其它]

一旦不再需要画笔，记得用DeleteObject函数将其删除



### SelectObject

 该函数选择一对象到指定的[设备上下文](https://baike.baidu.com/item/设备上下文/6768144)环境中，该新对象替换先前的相同类型的对象。 

```
HGDIOBJ SelectObject(HDC hdc, HGDIOBJ hgdiobj)
```

参数：

hdc：设备上下文环境的句柄。

hgdiobj：被选择的对象的句柄，该指定对象必须由如下的函数创建。

[位图](https://baike.baidu.com/item/位图)：CreateBitmap, CreateBitmapIndirect, CreateCompatible Bitmap, [CreateDIBitmap](https://baike.baidu.com/item/CreateDIBitmap), CreateDIBsection（只有内存[设备上下文](https://baike.baidu.com/item/设备上下文)环境可选择位图，并且在同一时刻只能一个设备上下文环境选择位图）。

画刷：[CreateBrushIndirect](https://baike.baidu.com/item/CreateBrushIndirect), [CreateDIBPatternBrush](https://baike.baidu.com/item/CreateDIBPatternBrush), [CreateDIBPatternBrushPt](https://baike.baidu.com/item/CreateDIBPatternBrushPt), CreateHatchBrush, CreatePatternBrush, CreateSolidBrush。

字体：CreateFont, CreateFontIndirect。

笔：CreatePen, CreatePenIndirect。

区域：[CombineRgn](https://baike.baidu.com/item/CombineRgn), [CreateEllipticRgn](https://baike.baidu.com/item/CreateEllipticRgn), [CreateEllipticRgnIndirect](https://baike.baidu.com/item/CreateEllipticRgnIndirect), [CreatePolygonRgn](https://baike.baidu.com/item/CreatePolygonRgn), CreateRectRgn, [CreateRectRgnIndirect](https://baike.baidu.com/item/CreateRectRgnIndirect)。

返回值：如果选择对象不是区域并且函数执行成功，那么返回值是被取代的对象的句柄；如果选择对象是区域并且函数执行成功，返回如下一值:

SIMPLEREGION：区域由单个矩形组成；

COMPLEXREGION：区域由多个矩形组成;

NULLREGION：区域为空。

如果发生错误并且选择对象不是一个区域，那么返回值为NULL，否则返回HGDI_ERROR。

注释：该函数返回先前指定类型的选择对象，一个应用程序在它使用新对象进行绘制完成之后，应该用原始的缺省的对象替换新对象。

应用程序不能同时选择一个位图到多个[设备上下文](https://baike.baidu.com/item/设备上下文)环境中。

ICM：如果被选择的对象是画笔或笔，那么就执行颜色管理。



###  GetStockObject

 该函数检索预定义的备用笔、刷子、字体或者[调色板](https://baike.baidu.com/item/调色板)的句柄。 

```
HGDIOBJ GetStockObject(int fnObject)；
```

fnObject：指定对象的类型，该参数可取如下值之一；

BLACK_BRUSH：黑色画刷；DKGRAY_BRUSH：暗灰色画刷；

DC_BRUSH：在[Windows98](https://baike.baidu.com/item/Windows98/5246326),Windows NT 5.0和以后版本中为纯颜色画刷，缺省色为白色，可以用[SetDCBrushColor](https://baike.baidu.com/item/SetDCBrushColor)函数改变颜色，更多的信息参见以下的注释部分。

GRAY_BRUSH：灰色画刷笔；

HOLLOW_BRUSH：空画刷（相当于NULL_BRUSH）；

NULL_BRUSH：空画刷（相当于HOLLOW_BRUSH）；

LTGRAY_BRUSH：亮灰色画刷；

WHITE_BRUSH：白色画刷；

BLACK_PEN：黑色钢笔；

DC_PEN：在[Windows98](https://baike.baidu.com/item/Windows98/5246326)、Windows NT 5.0和以后版本中为纯色钢笔，缺省色为白色，使用[SetDCPenColor](https://baike.baidu.com/item/SetDCPenColor)函数可以改变色彩，更多的信息，参见下面的注释部分。

WHITE_PEN：白色钢笔；

ANSI_FIXED_FONT：在Windows中为固定间距（等宽）系统字体；

ANSI_VAR_FONT：在Windows中为变间距（比例间距）系统字体；

DEVICE_DEFAUCT_FONT：在WindowsNT中为设备相关字体；

DEFAULT_GUI_FONT：用户界面对象缺省字体，如菜单和对话框；

OEM_FIXED_FONT：[原始设备制造商](https://baike.baidu.com/item/原始设备制造商)（OEM）相关固定间距（等宽）字体；

SYSTEM_FONT：系统字体，在缺省情况下，系统使用系统字体绘制菜单，对话框控制和文本；

SYSTEM_FIXED_FONT：固定间距（等宽）系统字体，该对象仅提供给兼容16位Windows版本；

DEFAULT_PALETTE：缺省[调色板](https://baike.baidu.com/item/调色板)，该调色板由系统调色板中的静态色彩组成。

#####    返回值

如果成功，返回值标识申请的逻辑对象，如果失败，返回值为NULL。

#####   注释

仅在CS_HREDRAW和CS_VREDRAW风格的窗口中使用DKGRAY_BRUSH、GRAY_BRUSH和LTGRAY_BRUSH对象。

如果在其他风格的窗口中使灰色画笔，可能导致在窗口移动或改变大小之后出现画笔模式错位现象，原始储存画笔不能被调整。

HOLLOW_BRUSH和NULL_BRUSH储存对象相等。

由DEFAULT_GUI_FONT储存对象使用的字体将改变。当想使用菜单、对话框和其他用户界面对象使用的字体时请使用此储存对象。

不必要通过调用[DeleteObject](https://baike.baidu.com/item/DeleteObject/6379900)函数来删除储存对象。

```
GetStockObject(BLACK_PEN);
```



### DeleteObject删除对象句柄

```
BOOL DeleteObject(HGDIOBJ hObject)；
```

hObject：逻辑笔、画笔、字体、[位图](https://baike.baidu.com/item/位图)、区域或者调色板的句柄。

返回值：成功，返回非零值；如果指定的句柄无效或者它已被选入[设备上下文](https://baike.baidu.com/item/设备上下文)环境，则返回值为零。

注释：当一个绘画对象（如笔或画笔）当前被选入一个设备上下文环境时不要删除该对象。当一个调色板画笔被删除时，与该画笔相关的位图并不被删除，该图必须单独地删除。

系统备用的画笔不可以DeleteObject



<hr>



```c
// 画笔.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "画笔.h"

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
	//创建画笔句柄变量
	static HPEN hPen1, hPen2, hPen3,hOldPen;
	static int cxClient, cyClient;
	static int cyLine;
    switch (message)
    {
	case WM_SIZE:
		cxClient = LOWORD(lParam);
		cyClient = HIWORD(lParam);
		cyLine   = cyClient / 8;
		//创建画笔句柄,除了PS_SOLID可以设置宽度,其他的都不可以设置宽度 CreatePen(, 宽度 ,)
		hPen1 = CreatePen(PS_SOLID,1,RGB(0,0,0));
		hPen2 = CreatePen(PS_DASH, 1, RGB(255, 0, 0));
		hPen3 = CreatePen(PS_DOT, 1, RGB(0, 255, 0));

		break;
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

			//SelectObject 设置画笔
			hOldPen = (HPEN)SelectObject(hdc, hPen1);
			//设置起点位置
			MoveToEx(hdc,0,cyLine * 1,NULL);
			//从起点画到终点
			LineTo(hdc,cxClient, cyLine * 1);

			SelectObject(hdc, hPen2);
			MoveToEx(hdc, 0, cyLine * 2, NULL);
			LineTo(hdc, cxClient, cyLine * 2);

			SelectObject(hdc, hPen3);
			MoveToEx(hdc, 0, cyLine * 3, NULL);
			LineTo(hdc, cxClient, cyLine * 3);

			SelectObject(hdc, CreatePen(PS_DASHDOT,1,RGB(20,25,255)));
			MoveToEx(hdc, 0, cyLine * 4, NULL);
			LineTo(hdc, cxClient, cyLine * 4);
			//DeleteObject删除旧画笔句柄
			DeleteObject(SelectObject(hdc, CreatePen(PS_DASHDOTDOT, 1, RGB(20, 25, 255))));
			MoveToEx(hdc, 0, cyLine * 5, NULL);
			LineTo(hdc, cxClient, cyLine * 5);

			//系统备用的画笔不可以DeleteObject
			//GetStockObject获取系统备用对象,获取备用画笔句柄
			DeleteObject(SelectObject(hdc,GetStockObject(BLACK_PEN) ));
			MoveToEx(hdc, 0, cyLine * 6, NULL);
			LineTo(hdc, cxClient, cyLine * 6);

			SelectObject(hdc,hOldPen);
			DeleteObject(hPen1);
			DeleteObject(hPen2);
			DeleteObject(hPen3);
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

