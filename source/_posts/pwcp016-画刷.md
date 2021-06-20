---
title: 画刷
date: 2021-06-13 00:25:11
tags:
categories: windows程序设计2
doc:
---

# 画刷

### 备用画刷（stock brush）

​		1.WHITE_BRUSH

白色画刷

2. LTGRAY_BRUSH

浅灰色画刷

 3.GRAY_BRUSH

灰色画刷

4.DKGRAY_BRUSH

深灰色画刷

5. BLACK_BRUSH

黑色画刷

6. NULL_BRUSH

空画刷

### 创建颜色画刷

```
 HBRUSH CreateSolidBrush(COLORREF crColor); 
```



```
CreateSolidBrush(RGB(255,100,200));
```



### 创建阴影画刷

```
HBRUSH CreateHatchBrush(int fnStyle, COLORREF clrref)；
```

fnStyle：指定刷子的阴影样式。该参数可以取下列值，这些值的含义为：

HS_BDIAGONAL：表示45度向上，从左至右的阴影(/////)；

HS_CROSS：水平和垂直交叉阴影(+++++)；

HS_DIAGCROSS：45度交叉阴影(XXXXX)；

HS_FDIAGONAL：45度向下，自左至右阴影(\\\\\\)；

HS_HORIZONTAL：水平阴影(-----)；

HS_VERTICAL：垂直阴影(|||||)。

cirref：指定用于阴影的刷子的前景色。

返回值：如果函数执行成功，那么返回值标识为逻辑刷子；如果函数执行失败，那么返回值为NULL。

```
CreateHatchBrush(HS_VERTICAL, RGB(2, 202, 33));
```



### HBRUSH类型

画刷句柄类型



<hr>

```c
// 画刷.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "画刷.h"

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
			HBRUSH hBrush;
            HDC hdc = BeginPaint(hWnd, &ps);
			SelectObject(hdc,CreatePen(PS_SOLID,5,RGB(255,0,0)));
			
			SelectObject(hdc, GetStockObject(GRAY_BRUSH));
			Rectangle(hdc,100,100,200,200);

			hBrush = (HBRUSH)CreateHatchBrush(HS_HORIZONTAL, RGB(2, 202, 33));
			SelectObject(hdc, hBrush);
			Rectangle(hdc, 250, 100, 350, 200);
			
			DeleteObject(SelectObject(hdc, CreateSolidBrush(RGB(255,100,200))));
			Rectangle(hdc, 400, 100, 500, 200);
			
			DeleteObject(SelectObject(hdc, CreateHatchBrush(HS_BDIAGONAL, RGB(2, 202, 33))));
			Rectangle(hdc, 550, 100, 650, 200);
			DeleteObject(SelectObject(hdc, CreateHatchBrush(HS_VERTICAL, RGB(2, 202, 33))));
			Rectangle(hdc, 700, 100, 800, 200);
			DeleteObject(SelectObject(hdc, GetStockObject(WHITE_BRUSH)));
			DeleteObject(SelectObject(hdc, GetStockObject(WHITE_PEN)));
            // TODO: 在此处添加使用 hdc 的任何绘图代码...
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

