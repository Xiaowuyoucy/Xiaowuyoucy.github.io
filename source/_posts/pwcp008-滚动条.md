---
title: 滚动条
date: 2021-06-01 12:05:13
tags:
categories: windows程序设计2
doc:
---

# 滚动条

1. 在CreateWindow中加入WS_VSCROLL
2. 添加WM_VSCROLL消息
3. 设置滑块大小
4. 设置滑块位置
5. 更新客户区

### WS_VSCROLL垂直滚动条

```c
   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW | WS_VSCROLL,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);
```

### WS_HSCROLL水平滚动条

```c
   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW | WS_HSCROLL,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);
```

### WM_VSCROLL消息

SB_PAGEDOWN 向下滚动一页

SB_LINEDOWN 向下滚动一行

SB_PAGEUP 向上滚动一页

SB_LINEUP 向上滚动一行

SB_TOP 滚动到顶端

SB_BOTTOM 滚动到底部

SB_THUMBPOSITION 用户有拖动滚动框 （滑块），并释放鼠标按钮。HIWORD 指示在拖动操作结束时滚动框的位置。

SB_THUMBTRACK 用户正在拖动滚动框。直到用户释放鼠标按钮，反复发送此邮件。HIWORD 指示滚动框已被拖动到的位置。

```c
case WM_VSCROLL:
		switch (LOWORD(wParam)) //取wParam低位
		{
		case SB_LINEUP:		//向上滚动一行
			iVscrollPos -= 1;
			break;
		case SB_LINEDOWN: //向下滚动一行
			iVscrollPos += 1;
			break;
		case SB_PAGEDOWN:	//向下滚动一页
			iVscrollPos += cyClient / cyChar;
			break;
		case SB_PAGEUP:		//向上滚动一页
			iVscrollPos -= cyClient / cyChar;
			break;
		case SB_THUMBTRACK:		//按下鼠标拖动滑块
			iVscrollPos = HIWORD(wParam);//获取高位,滑块的位置
			break;
		}
		iVscrollPos = max(0, min(iVscrollPos, MAX_LINE - 1));
		//如果不等于当前滑块位置
		if(iVscrollPos != GetScrollPos(hWnd,SB_VERT))
			SetScrollPos(hWnd, SB_VERT, iVscrollPos, TRUE);
		InvalidateRect(hWnd,NULL,TRUE);

		break;
```

### SetScrollRange

 函数设置所指定滚动条范围的最小值和最大值 

```
BOOL SetScrollRange(
HWND hWnd, // 窗口句柄
int nBar, // 滚动条类型
int nMinPos, // 滚动条的最小位置
int nMaxPos, // 滚动条的最大位置
BOOL bRedraw // 重绘标志
);
```

```
SetScrollRange(hWnd,SB_VERT,0, MAX_LINE - 1,FALSE);
```



### GetScrollPos

 获取指定滚动条中滚动按钮的当前位置 

hWnd：根据参数nBar值，处理滚动条控制或带有标准滚动条[窗体](https://baike.baidu.com/item/窗体)。

nBar：指定滚动条将被检查。

SB_HORZ：水平滚动条

SB_VERT：垂直滚动条

```
GetScrollPos(hWnd,SB_VERT);
```



### SetScrollPos

 用于设置所指定滚动条中的滚动按钮的位置。 

```
int SetScrollPos(
HWNDhWnd, //窗体句柄
intnBar, //滚动条
intnPos, //滚动条的新位置
BOOLbRedraw // 重绘标志
);
```

SB_HORZ：水平滚动条

SB_VERT：垂直滚动条

```
SetScrollPos(hWnd, SB_VERT, iVscrollPos, TRUE);
```



### InvalidateRect

使整个窗口无效

一般用来重画窗口,触发WM_PAINT消息

 向指定的[窗体](https://baike.baidu.com/item/窗体/4163553)更新区域添加一个矩形，然后窗体跟新区域的这一部分将被重新绘制。 

```
BOOL InvalidateRect(
HWND hWnd, // 窗口句柄
CONST RECT *lpRect, // 矩形指针变量
BOOL bErase //是否重画
);
```

 lpRect：如果为NULL，全部的窗口客户区域将被增加到更新区域中。 

### UpdateWindow

 更新指定窗口的客户区 

```
BOOL UpdateWindow(
HWND hWnd // 窗口的句柄
);
```



### WM_SIZE

当窗口大小发生改变时触发消息

```c
	case WM_SIZE:
		//获取客户区的高度
		cyClient = HIWORD(lParam);
		//获取客户区的宽度
		cxClient = LOWORD(lParam);
		break;
```





```c
// 滚动条.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "滚动条.h"

#define MAX_LOADSTRING 100
#define MAX_LINE 100
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
	wcex.lpszMenuName	= NULL;// MAKEINTRESOURCEW(IDC_MY);
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
   //WS_VSCROLL 创建垂直滚动条
   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW | WS_VSCROLL,
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
	static TEXTMETRIC tm;			//获取字体信息
	static int cxChar;				//字体宽度
	static int cyChar;				//字体高度
	HDC hdc;
	static int iVscrollPos;			//当前滑块位置
	TCHAR szBuffer[1024] = { 0 };
	static int cyClient;			//当前客户区高度大小
	static int cxClient;			//当前客户区宽度大小
    switch (message)
    {
	case WM_SIZE:
		//获取客户区的高度
		cyClient = HIWORD(lParam);
		//获取客户区的宽度
		cxClient = LOWORD(lParam);
		break;
	case WM_VSCROLL:
		//获取滚动条操作的消息
		switch (LOWORD(wParam))
		{
			//向上滚动一行
		case SB_LINEUP:
			iVscrollPos -= 1;
			break;
			//向下滚动一行
		case SB_LINEDOWN:
			iVscrollPos += 1;
			break;
			//向下滚动一页
		case SB_PAGEDOWN:
			iVscrollPos += cyClient / cyChar;
			break;
			//向上滚动一页
		case SB_PAGEUP:
			iVscrollPos -= cyClient / cyChar;
			break;
			//拖动滑块时
		case SB_THUMBTRACK:
			//拖动滑块时,获取滑块位置,在wParam的高位
			iVscrollPos = HIWORD(wParam);
			break;
		}
		//最小是0,最大是MAX_LINE - 1
		iVscrollPos = max(0, min(iVscrollPos, MAX_LINE - 1));
		//如果当前滑块位置不等于iVscrollPos
		if(iVscrollPos != GetScrollPos(hWnd,SB_VERT))
			SetScrollPos(hWnd, SB_VERT, iVscrollPos, TRUE);
		InvalidateRect(hWnd,NULL,TRUE);

		break;
	case WM_CREATE:
		hdc = GetDC(hWnd);
		GetTextMetrics(hdc, &tm);
		cxChar = tm.tmAveCharWidth;
		cyChar = tm.tmHeight + tm.tmExternalLeading;
		ReleaseDC(hWnd,hdc);
		//当前滑块默认为0
		iVscrollPos = 0;
		//设置滚动条大小
		SetScrollRange(hWnd,SB_VERT,0, MAX_LINE - 1,FALSE);
		//设置滑块位置
		SetScrollPos(hWnd,SB_VERT,iVscrollPos,TRUE);

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
			int x;
			int y;
            HDC hdc = BeginPaint(hWnd, &ps);
            // TODO: 在此处添加使用 hdc 的任何绘图代码...
			for (int i = 0, temp; i < MAX_LINE; i++)
			{
				 x = 0;
				 if (iVscrollPos >= MAX_LINE - cyClient/cyChar)
				 {
					 y = (i - (MAX_LINE - cyClient / cyChar)) * cyChar;
					 //SetScrollPos(hWnd, SB_VERT, y, TRUE);
				 }
				 else {
					 y = (i - iVscrollPos) * cyChar;
				 }


				_sntprintf(szBuffer,1024,TEXT("Hello %d "),i + 1);
				TextOut(hdc, x, y,szBuffer,lstrlen(szBuffer));
			}

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

