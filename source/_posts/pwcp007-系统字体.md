---
title: 系统字体
date: 2021-05-31 19:26:57
tags:
categories: windows程序设计2
doc:
---

# 系统字体

![1622460534446](/images/javawz/1622460534446.png) 

### TEXTMETRIC字体信息结构

```c
typedef struct tagTEXTMETRIC { 
LONG tmHeight; //字符高度
LONG tmAscent; //字符上部高度(基线以上)
LONG tmDescent; //字符下部高度(基线以下)
LONG tmInternalLeading, //由tmHeight定义的字符高度的顶部空间数目
LONG tmExternalLeading, //夹在两行之间的空间数目
LONG tmAveCharWidth, //平均字符宽度
LONG tmMaxCharWidth, //最宽字符的宽度
LONG tmWeight; //字体的粗细轻重程度
LONG tmOverhang, //加入某些拼接字体上的附加高度
LONG tmDigitizedAspectX, //字体设计所针对的设备水平方向
LONG tmDigitizedAspectY, //字体设计所针对的设备垂直方向
BCHAR tmFirstChar; //为字体定义的第一个字符
BCHAR tmLastChar; //为字体定义的最后一个字符
BCHAR tmDefaultChar; //字体中所没有字符的替代字符
BCHAR tmBreakChar; //用于拆字的字符
BYTE tmItalic, //字体为斜体时非零
BYTE tmUnderlined, //字体为下划线时非零
BYTE tmStruckOut, //字体被删去时非零
BYTE tmPitchAndFamily, //字体间距(低4位)和族(高4位)
BYTE tmCharSet; //字体的字符集
} TEXTMETRIC;
```





### GetTextMetrics

 把程序当前的字体信息，存放到[TEXTMETRIC](https://baike.baidu.com/item/TEXTMETRIC/10316529) 

```
BOOL GetTextMetrics(HDC hdc, LPTEXTMETRIC lptm)；
```

hdc：设备环境句柄。

lptm：指向结构TEXTMETRIC的[指针](https://baike.baidu.com/item/指针)，该结构用于获得字体信息。

```
GetTextMetrics(hdc, &tm);
```

###  

### GetSystemMetrics



```
int WINAPI GetSystemMetrics( __in intnIndex);
```

 只有一个参数，称之为「索引」，这个索引有75个[标识符](https://baike.baidu.com/item/标识符/7105638)，通过设置不同的标识符就可以获取系统分辨率、[窗体](https://baike.baidu.com/item/窗体/4163553)显示区域的宽度和高度、滚动条的宽度和高度。 

		//获取屏幕宽度
		cxScreen = GetSystemMetrics(SM_CXSCREEN);
		//获取屏幕高度
		cyScreen = GetSystemMetrics(SM_CYSCREEN);


### SetTextAlign

 为指定设备环境设置文字对齐标志 

```
UINT SetTextAlign(
HDC hdc, // 设备环境句柄
UINT fMode // 文本对齐选项
);
```

设置对齐方式为左对齐

	SetTextAlign(hdc, TA_LEFT | TA_TOP);
设置对齐方式为右对齐

```
SetTextAlign(hdc, TA_RIGHT | TA_TOP);
```



<hr>

```c
// 系统字体.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "系统字体.h"

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
	wcex.lpszMenuName	= NULL;//MAKEINTRESOURCEW(IDC_MY);
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
	static TEXTMETRIC tm;
	static int cxChar;	//字符宽度
	static int cyChar;	//字符高度
	static int cxCap;	//一个字符的宽度
	static int cxScreen, cyScreen;//屏幕宽度和高度
	TCHAR szBuffer[1024] = { 0 };
	int len;	//字符串长度
	HDC hdc;
    switch (message)
    {
	case WM_CREATE:
		hdc = GetDC(hWnd);

		//把程序当前的字体信息，存放到tm
		GetTextMetrics(hdc, &tm);
		//获取字符平均值
		cxChar = tm.tmAveCharWidth;
		//获取字符高度
		cyChar = tm.tmHeight + tm.tmExternalLeading + 10;
		//获取一个字符的宽度
		cxCap = (tm.tmPitchAndFamily & 1 ? 3 : 2) * cxChar / 2;
		//获取屏幕宽度
		cxScreen = GetSystemMetrics(SM_CXSCREEN);
		//获取屏幕高度
		cyScreen = GetSystemMetrics(SM_CYSCREEN);
		ReleaseDC(hWnd,hdc);
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
			TextOut(hdc,0,0,TEXT("SM_CXSCREEN"),lstrlen(TEXT("SM_CXSCREEN")));

			TextOut(hdc, cxCap * 30, 0, TEXT("屏幕宽度"), lstrlen(TEXT("屏幕宽度")));
			//设置对齐方式为右对齐
			SetTextAlign(hdc,TA_RIGHT|TA_TOP);
			len = _sntprintf(szBuffer,1024,TEXT("%d"),cxScreen);
			TextOut(hdc, cxCap * 60, 0, szBuffer,len);
			//设置对齐方式为左对齐
			SetTextAlign(hdc,TA_LEFT | TA_TOP);
			TextOut(hdc, 0, cyChar, TEXT("SM_CYSCREEN"), lstrlen(TEXT("SM_CYSCREEN")));

			TextOut(hdc, cxCap * 30, cyChar, TEXT("屏幕高度"), lstrlen(TEXT("屏幕高度")));
			//设置对齐方式为右对齐
			SetTextAlign(hdc, TA_RIGHT | TA_TOP);

			len = _sntprintf(szBuffer, 1024, TEXT("%d"), cyScreen);
			TextOut(hdc, cxCap * 60, cyChar, szBuffer, len);
			//设置对齐方式为左对齐
			SetTextAlign(hdc, TA_LEFT | TA_TOP);
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

