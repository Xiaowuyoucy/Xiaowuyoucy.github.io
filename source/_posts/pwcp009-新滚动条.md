---
title: 新滚动条
date: 2021-06-02 16:05:19
tags:
categories: windows程序设计2
doc:
---

# 新滚动条

### SCROLLINFO

```
typedef struct tagSCROLLINFO
{
UINT cbSize ; // set to sizeof (SCROLLINFO)
UINT fMask ; // values to set or get
int nMin; // 滚动条最小值
int nMax; // 滚动条最大值
UINT nPage ; // 一页大小
int nPos ; 
int nTrackPos ; // 当前滑块位置
}SCROLLINFO,*LPSCROLLINFO;
```



### SetScrollInfo

 设置滚动条参数 

```
int setscrollinfo(hwnd hwnd,int fnbar,lpscrollinfo lpsi,bool fredraw);
```

参数：

hWnd：滚动条控件或带标准滚动条的[窗体](https://baike.baidu.com/item/窗体)句柄，由fnBar参数决定。

fnBar：指定被设定参数的滚动条的类型。这个参数可以是下面值，含义如下：

SB_CTL：设置滚动条控件。而参数hwnd必须是滚动条控件的句柄。

SB_HORZ：设置所给定的[窗体](https://baike.baidu.com/item/窗体)上标准水平滚动条参数。

SB_VERT：设置所给定的[窗体](https://baike.baidu.com/item/窗体)上标准垂直滚动条参数。

lpsi：指向SCROLLINFO结构。在调用SetScrollInfo之前，设置[SCROLLINFO](https://baike.baidu.com/item/SCROLLINFO)结构中cbSize成员以标识结构大小，设置成员fMask以说明待设置的滚动条参数，并且在适当的成员中制定新的参数值。成员fMask可以为下面所列复合值，含义如下：

SIF_DISABLENOSCROLL：如果滚动条的新参数使其为没必要，则使滚动条无效而不再移动它。

SIF_PAGE：设置滚动页码值到由Ipsi指向的SCROLLINFO结构的nPage成员中。

SIF_POS：设置滚动位置值到由lpsi指向的SCROLLINFO结构的nPos成员中。

SIF_RANGE：设置滚动范围值到由lpsl指向的SCROLLINFO结构的nMin和nMax成员中。

fRedraw：指定滚动条是否重画以反映滚动条的变化。如果这个参数为TRUE，滚动条将被重画，否则不被重画。

```c
//设置SIF_POS
si.fMask = SIF_POS;
//设置滑块位置
SetScrollInfo(hWnd,SB_VERT,&si,TRUE);
```



### GetScrollInfo

 获取滚动条的参数 

```
BOOL GetScrollInfo( HWND hWnd, int fnBar, LPSCROLLINFO lpsi );
```

**参数**：

hWnd：滚动条控制或有标准滚动条的[窗体](https://baike.baidu.com/item/窗体)句柄，由fnBar参数确定。

fnBar：指定待找回滚动条参数的类型，此参数可以为如下值，其值含义：

SB_CTL：找回滚动条控制参数。其中参数hwnd一定是处理滚动条控制的句柄。

SB_HORZ：找回所指定[窗体](https://baike.baidu.com/item/窗体)的标准水平滚动条参数。

SB_VERT：找回所指定[窗体](https://baike.baidu.com/item/窗体)的标准垂直滚动条参数。

lpsi：指向SCROLLINFO结构。

```c
GetScrollInfo(hWnd, SB_VERT, &si);
```



### ScrollWindow

 函数滚动所指定的窗口客户区域内容。 

```
BOOL ScrollWindow(HWND hWnd, int XAmount, int YAmount, CONST RECT *IpRect, CONST RECT *lpClipRect);
```

hWnd

[in]客户区域将被滚动的窗口的[句柄](https://baike.baidu.com/item/句柄)。

XAmount

[in]指定水平滚动的距离，以设备单位计。如果[窗口类](https://baike.baidu.com/item/窗口类)风格为CS_OWNDC或CS_CLASSDC，则此参数则使用逻辑单位而非设备单位。当向左滚动[窗体](https://baike.baidu.com/item/窗体)内容时，参数值必须为负。

YAmount

[in]指定垂直滚动的距离，以设备单位计。如果窗口类风格为CS_OWNDC或CS_CLASSDC，则此参数则使用逻辑单位而非设备单位。当向上滚动[窗体](https://baike.baidu.com/item/窗体)内容时，参数值必须为负。

lpRect

[in]指向[RECT](https://baike.baidu.com/item/RECT)结构的[指针](https://baike.baidu.com/item/指针)，该结构指定了将要滚动的客户区范围。若此参数为NULL，则整个客户区域将被滚动。

lpClipRect

[in]指向[RECT](https://baike.baidu.com/item/RECT)结构的指针，该结构指定了要滚动的[裁剪区域](https://baike.baidu.com/item/裁剪区域)。只有这个矩形中的位才会被滚动。在矩形之外的位不会被影响，即使它们是在lpRect矩形之内。(见代码"测试一")假如lpClipRect为NULL，则不会在滚动矩形上进行裁剪。





```c
ScrollWindow(hWnd, 0, cyChar * (iVertPos - si.nPos),NULL,NULL);
```

![1622623528697](/images/javawz/1622623528697.png)

![1622623600959](/images/javawz/1622623600959.png)

<br>

<br>

<br>

<br>

<br>

<hr>









```c
// 滚动条2.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "滚动条2.h"

#define MAX_LOADSTRING 100
#define MAX_LINE 1024
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
    LoadStringW(hInstance, IDC_MY2, szWindowClass, MAX_LOADSTRING);
    MyRegisterClass(hInstance);

    // 执行应用程序初始化: 
    if (!InitInstance (hInstance, nCmdShow))
    {
        return FALSE;
    }

    HACCEL hAccelTable = LoadAccelerators(hInstance, MAKEINTRESOURCE(IDC_MY2));

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
    wcex.hIcon          = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_MY2));
    wcex.hCursor        = LoadCursor(nullptr, IDC_ARROW);
    wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
	wcex.lpszMenuName	= NULL;// MAKEINTRESOURCEW(IDC_MY2);
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
	SCROLLINFO si;

	static TEXTMETRIC tm;			//获取字体信息
	static int cxChar;				//字体宽度
	static int cyChar;				//字体高度
	HDC hdc;
	static int iVscrollPos;			//当前滑块位置
	static TCHAR szBuffer[1024][1024] = { 0 };
	static int cyClient;			//当前客户区高度大小
	static int cxClient;			//当前客户区宽度大小
	static int iVertPos;
	static int iPaintBeg;
	static int iPaintEnd;
	switch (message)
	{
	case WM_SIZE:
		//获取客户区的高度
		cyClient = HIWORD(lParam);
		//获取客户区的宽度
		cxClient = LOWORD(lParam);
		//初始化si结构大小
		si.cbSize = sizeof(si);
		//设置可用标志
		//SIF_RANGE:使用nMin和nMax成员
		//SIF_PAGE:使用nPage成员
		si.fMask = SIF_RANGE | SIF_PAGE;
		//设置一页大小
		si.nPage = cyClient / cyChar;
		//设置滚动条最小值
		si.nMin = 0;
		//设置滚动条最大值
		si.nMax = MAX_LINE - 1;
		//设置滚动条参数
		SetScrollInfo(hWnd,SB_VERT,&si,TRUE);
		//窗口设置为失效
		InvalidateRect(hWnd, NULL, TRUE);
		//更新窗口
		UpdateWindow(hWnd);
		break;
	case WM_VSCROLL:

		si.cbSize = sizeof(si);
		//SIF_ALL:所有成员可用
		si.fMask = SIF_ALL;
		//获取滚动条信息
		GetScrollInfo(hWnd,SB_VERT,&si);
		iVertPos = si.nPos;
		//获取滚动条操作的消息
		switch (LOWORD(wParam))
		{
			//向上滚动一行
		case SB_LINEUP:
			si.nPos -= 1;
			break;
			//向下滚动一行
		case SB_LINEDOWN:
			si.nPos += 1;
			break;
			//向下滚动一页
		case SB_PAGEDOWN:
			si.nPos += si.nPage;
			break;
			//向上滚动一页
		case SB_PAGEUP:
			si.nPos -= si.nPage;
			break;
			//拖动滑块时
		case SB_THUMBTRACK:
			
			si.nPos = si.nTrackPos;
			break;
		}

		//设置SIF_POS
		si.fMask = SIF_POS;
		//设置滑块位置
		SetScrollInfo(hWnd,SB_VERT,&si,TRUE);
		//获取滚动条信息
		GetScrollInfo(hWnd, SB_VERT, &si);
		//如果滑块位置不等于当前滑块位置
		if (si.nPos != iVertPos)
		{
			//滚动所指定的窗口客户区域内容
			ScrollWindow(hWnd, 0, cyChar * (iVertPos - si.nPos),NULL,NULL);
			UpdateWindow(hWnd);
		}




		break;
	case WM_CREATE:
		hdc = GetDC(hWnd);
		GetTextMetrics(hdc, &tm);
		cxChar = tm.tmAveCharWidth;
		cyChar = tm.tmHeight + tm.tmExternalLeading;
		ReleaseDC(hWnd, hdc);
		
		for (int i = 0; i < MAX_LINE; i++)
		{
			_sntprintf(szBuffer[i], 1024, TEXT("Hello %d "), i + 1);
		}


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
		
		si.cbSize = sizeof si;
		si.fMask = SIF_ALL;
		GetScrollInfo(hWnd,SB_VERT,&si);
		iVertPos = si.nPos;
		//获取滚动条开始打印的位置
		//ps.rcPaint.top 无效矩形的顶位置
		iPaintBeg = max(0, iVertPos + ps.rcPaint.top / cyChar);
		//获取滚动条结束打印的位置
		//ps.rcPaint.bottom无效矩形的底位置
		iPaintEnd = min(MAX_LINE - 1, iVertPos + ps.rcPaint.bottom / cyChar);


		// TODO: 在此处添加使用 hdc 的任何绘图代码...
		for (int i = iPaintBeg, temp; i < iPaintEnd; i++)
		{
			x = 0;
			y = (i - iVertPos) * cyChar;
			//SetScrollPos(hWnd, SB_VERT, y, TRUE);

			
			TextOut(hdc, x, y, szBuffer[i], lstrlen(szBuffer[i]));
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

