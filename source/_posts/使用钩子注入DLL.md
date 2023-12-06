---
title: 使用钩子注入DLL
date: 2023-12-04 22:47:56
tags:
categories: windows程序设计2
doc:
---



原理：通过SetWindowsHookEx函数将DLL注入到进程的地址空间中，最后一个参数dwThreadId指向的是被注入进程内的某个线程ID。
(1)    进程A对线程dwThread挂键盘钩子
(2)    线程dwThreadId获取到的键盘消息会实现被钩子拦截
(3)    系统检查hMod指向的DLL是否已被载入到线程dwThreadId所在的进程地址空间中，若否，则载入。这时，假设DLL被载入到进程B
(4)    系统在进程B的地址空间中调用lpfn函数



钩子的回调函数格式

```
LRESULT CALLBACK FunProc(
	int code,       // hook code
	WPARAM wParam,  // virtual-key code
	LPARAM lParam   // keystroke-message information
	)
```



消息框

```
#include <windows.h>

/*
 * @brief 显示一个消息框，并等待用户的响应。
 * 
 * @param hwndParent 父窗口的句柄，如果为nullptr，则消息框将居中显示在屏幕上。
 * @param lpText 要显示的消息文本。
 * @param lpCaption 消息框标题。
 * @param uType 消息框的类型，可以是以下值的组合：
 *   - MB_ABORTRETRYIGNORE
 *   - MB_CANCELTRYCONTINUE
 *   - MB_HELP
 *   - MB_OK
 *   - MB_OKCANCEL
 *   - MB_RETRYCANCEL
 *   - MB_YESNO
 *   - MB_YESNOCANCEL
 * @return 用户的响应。可能的值包括：
 *   - IDABORT
 *   - IDCANCEL
 *   - IDCONTINUE
 *   - IDIGNORE
 *   - IDNO
 *   - IDOK
 *   - IDRETRY
 *   - IDTRYAGAIN
 *   - IDYES
 */
int MessageBox(
    HWND hwndParent,
    LPCTSTR lpText,
    LPCTSTR lpCaption,
    UINT uType
);

```





#### CallNextHookEx

```
#include <windows.h>

/**
 * @brief 调用下一个钩子程序在钩子链中。
 * 
 * @param hhk 钩子句柄，由SetWindowsHookEx函数返回。
 * @param nCode 钩子代码，表示事件类型。具体取值取决于使用的钩子类型。
 * @param wParam 与事件相关的附加信息，具体含义取决于使用的钩子类型。
 * @param lParam 与事件相关的附加信息，具体含义取决于使用的钩子类型。
 * @return 下一个钩子程序的返回值，具体含义取决于使用的钩子类型。
 */
LRESULT CallNextHookEx(
    HHOOK hhk,
    int nCode,
    WPARAM wParam,
    LPARAM lParam
);


CallNextHookEx是Windows API中用于调用下一个钩子程序的函数。在使用钩子函数时，它允许当前的钩子程序调用下一个钩子程序，以确保整个钩子链能够正常运行。

参数说明：

hhk：钩子句柄，由 SetWindowsHookEx 函数返回。
nCode：钩子代码，表示事件类型。具体取值取决于使用的钩子类型。
wParam：与事件相关的附加信息，具体含义取决于使用的钩子类型。
lParam：与事件相关的附加信息，具体含义取决于使用的钩子类型。
返回值：
CallNextHookEx 的返回值取决于使用的钩子类型，通常是下一个钩子程序的返回值。

```



#### FindWindow

```
#include <windows.h>

/**
 * @brief 根据窗口类名和窗口标题查找顶层窗口的句柄。
 * 
 * @param lpClassName 窗口类名，如果为NULL，则匹配所有类名。
 * @param lpWindowName 窗口标题，如果为NULL，则匹配所有标题。
 * @return 找到的窗口的句柄，如果未找到则为NULL。
 */
HWND FindWindow(
    LPCSTR lpClassName,
    LPCSTR lpWindowName
);


lpClassName：窗口类名，如果为NULL，则匹配所有类名。
lpWindowName：窗口标题，如果为NULL，则匹配所有标题。
返回值：找到的窗口的句柄，如果未找到则为NULL。

```



#### GetWindowThreadProcessId

```
#include <windows.h>

/**
 * @brief 获取指定窗口的线程标识符和进程标识符。
 * 
 * @param hWnd 目标窗口的句柄。
 * @param lpdwProcessId 接收进程标识符的指针，如果为NULL则不检索。
 * @return 返回目标窗口所在的线程标识符。
 */
DWORD GetWindowThreadProcessId(
    HWND hWnd,
    LPDWORD lpdwProcessId
);

hWnd：目标窗口的句柄。
lpdwProcessId：接收进程标识符的指针。如果为NULL，则不检索进程标识符。
返回值：返回目标窗口所在的线程标识符。
该函数允许您获取与指定窗口相关联的线程标识符和进程标识符。可以使用这些标识符来进行诸如线程注入和进程间通信等操作。

```



#### SetWindowsHookEx

```
#include <windows.h>

/**
 * @brief 安装一个钩子函数来监视指定类型的事件。
 * 
 * @param idHook 要安装的钩子类型。
 * @param lpfn 钩子过程的地址。
 * @param hMod 包含钩子过程的 DLL 的句柄。如果为NULL，则将使用当前进程的句柄。
 * @param dwThreadId 与要监视的事件相关联的线程的标识符。如果为0，则表示安装全局钩子。
 * @return 成功时返回钩子的句柄，失败时返回NULL。
 */
HHOOK SetWindowsHookEx(
    int idHook,
    HOOKPROC lpfn,
    HINSTANCE hMod,
    DWORD dwThreadId
);



其中idHook参数可以取如下常量：
WH_CALLWNDPROC     //窗口钩子，当系统向目标窗口发送消息时将触发此钩子
WH_CALLWNDPROCRET     //窗口钩子，当窗口处理完消息后将触发此钩子
WH_CBT   //当Windows激活、产生、释放（关闭）、最小化、最大化或改变窗口时都将触发此事件
WH_DEBUG    //调试钩子
WH_GETMESSAGE    //当往消息队列中增加一个消息时将触发此钩子
WH_JOURNALPLAYBACK     //回放钩子，可以用于播放已记录的鼠标和键盘的操作
WH_JOURNALRECORD     //记录钩子，可以用于记录鼠标和键盘的操作，木马程序可以使用此钩子窃取受控方在屏幕中敲入的密码
WH_KEYBOARD     //当敲击键盘时将触发此钩子
WH_MOUSE    //当有鼠标操作时将触发此钩子
WH_MSGFILTER   //消息过滤钩子
WH_SHELL   //Shell钩子
WH_SYSMSGFILTER    //系统消息过滤钩子
其他特定类型的钩子，具体取决于您的需求。

lpfn：指向钩子过程的函数指针。此函数在发生事件时被调用。
hMod：包含钩子过程的 DLL 的句柄。如果为NULL，则将使用当前进程的句柄。
dwThreadId：与要监视的事件相关联的线程的标识符。如果为0，则表示安装全局钩子，针对所有线程的事件。
返回值：成功时返回钩子的句柄，失败时返回NULL。
SetWindowsHookEx函数用于安装一个钩子，允许您监视并拦截指定类型的事件。成功安装后，钩子过程将在事件发生时被调用。通常，钩子过程是在DLL中实现的，以便可以在不同进程间共享。
```





#### UnhookWindowsHookEx

```
#include <windows.h>

/**
 * @brief 卸载之前安装的钩子。
 * 
 * @param hhk 先前安装的钩子的句柄。
 * @return 如果成功，返回非零值；如果失败，返回零。
 */
BOOL UnhookWindowsHookEx(
    HHOOK hhk
);


hhk：先前安装的钩子的句柄，由 SetWindowsHookEx 返回。
返回值：如果成功，返回非零值；如果失败，返回零。
UnhookWindowsHookEx函数用于卸载之前通过 SetWindowsHookEx 安装的钩子。在不再需要钩子时，调用此函数以确保释放相关资源，并停止钩子过程的调用。成功卸载后，钩子句柄将不再有效。
```













```
#include <Windows.h>

LRESULT CALLBACK FunProc(
	int code,       // hook code
	WPARAM wParam,  // virtual-key code
	LPARAM lParam   // keystroke-message information
	)
{
	MessageBox(NULL,"KEY PRESS","hook inject",MB_OK);
	return CallNextHookEx(0,code,wParam,lParam);
}

HHOOK g_HookHandle;

__declspec(dllexport) void SetHook()
{
	DWORD tid = 0;
	// 获取窗口句柄
	HWND gameh = FindWindow(NULL,"test.txt - 记事本");
	if (gameh == 0)
	{
		return;
	}
	// 获取创建这个窗口的线程
	tid = GetWindowThreadProcessId(gameh,NULL);
	// 安装钩子到指定线程 WH_KEYBOARD 键盘钩子
	g_HookHandle = SetWindowsHookEx(WH_KEYBOARD, FunProc, GetModuleHandle("HOOKInject.dll"),tid);
}

__declspec(dllexport) void UnHook()  
{  
	UnhookWindowsHookEx(g_HookHandle);  
}  
```





```
#include <stdio.h>
#include <windows.h>

typedef void (*lpFun)();
int main()
{
	HINSTANCE hDll; //DLL句柄 
	lpFun SetHook;  //函数指针
	lpFun UnHook;   //函数指针
	hDll = LoadLibrary("..\\Debug\\HOOKInject.dll");
	if (hDll != NULL)
	{
		SetHook = (lpFun)GetProcAddress(hDll, "SetHook");
		UnHook = (lpFun)GetProcAddress(hDll, "UnHook");
	}

	if (SetHook != NULL)
	{
		SetHook();
	}
	getchar();
	UnHook();
	if (hDll != NULL)
	{
		FreeLibrary(hDll);
	}

	return 0;
}
```

