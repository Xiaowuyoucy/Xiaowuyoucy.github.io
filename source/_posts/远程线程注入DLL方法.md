---
title: 远程线程注入DLL方法
date: 2023-12-05 00:43:56
tags:
categories: windows程序设计2
doc:
---





原理：在目标进程中开启一个线程以执行LoadLibrary(Ex)操作。
步骤：
(1)   提升加载进程的权限
(1)   用VirtualAllocEx函数在远程进程的地址空间中分配一块内存存放参数
(2)   用WriteProcessMemory函数把DLL的路径名复制到(1)步骤分配的内存中
(3)   用GetProcAddress获取LoadLibrary(Ex)函数的实际地址
(4)   用CreateRemoteThread函数在远程进程中创建一个线程，让新线程调用正确的LoadLibrary(Ex)函数并在参数中传入(1)步骤分配的内存地址
(5)   用GetProcAddress获取FreeLibrary函数的实际地址
(6)   用CreateRemoteThread函数在远程进程中创建一个线程,让新线程调用正确的FreeLibrary函数并在参数中传入远程DLL的HMODULE
(7)   用VirtualFreeEx来释放(1)步骤分配的内存







```
// 远程线程注入DLL
#include <windows.h>
#include <stdio.h>

#define INJECTDLLNAME "HookAPIDLL1.dll"

//提升当前进程权限
void AdjustProcessTokenPrivilege()
{
	LUID luidTmp;
	HANDLE hToken;
	TOKEN_PRIVILEGES tkp;
	if (!OpenProcessToken(GetCurrentProcess(), TOKEN_ADJUST_PRIVILEGES | TOKEN_QUERY, &hToken))
	{
		OutputDebugString("AdjustProcessTokenPrivilege OpenProcessToken Failed ! \n");
		return;
	}
	if (!LookupPrivilegeValue(NULL, SE_DEBUG_NAME, &luidTmp))
	{
		OutputDebugString("AdjustProcessTokenPrivilege LookupPrivilegeValue Failed ! \n");
		CloseHandle(hToken);
		return;
	}
	tkp.PrivilegeCount = 1;
	tkp.Privileges[0].Luid = luidTmp;
	tkp.Privileges[0].Attributes = SE_PRIVILEGE_ENABLED;
	if (!AdjustTokenPrivileges(hToken, FALSE, &tkp, sizeof(tkp), NULL, NULL))
	{
		OutputDebugString("AdjustProcessTokenPrivilege AdjustTokenPrivileges Failed ! \n");
		CloseHandle(hToken);
		return;
	}
	return;
}

void GetExePath(char* pExePath) 
{     
	int  pathlen = GetModuleFileName(NULL, pExePath, MAX_PATH);    
	while(1)     
	{         
		if(pExePath[pathlen--]=='\\')             
			break;     
	}     
	pExePath[++pathlen] = 0;     
}   

int main(int argc,char **argv)
{
	CHAR* pDllName = NULL;
	HANDLE hProcess = NULL;
	DWORD dwProcessId = 0;
	LPVOID lpStartAddress = NULL;
	LPVOID lpParaAddress = NULL;
	HANDLE hRemoteThread = NULL;
	DWORD dwExitHandle = 0;

	// 获取一下DLL的路径
	CHAR tmpPath[MAX_PATH] = {0};
	GetExePath(tmpPath);
	strcat(tmpPath,"\\");
	strcat(tmpPath,INJECTDLLNAME);
	pDllName = tmpPath;
	printf("注入的DLL路径:%s \r\n", pDllName);

	printf("输入要注入的进程ID:");
	scanf("%d", &dwProcessId);

	// 提升当前进程的权限
	AdjustProcessTokenPrivilege();
	// 打开目标进程
	hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, dwProcessId);

	// 为目标进程分配内存
	lpParaAddress = VirtualAllocEx(hProcess, NULL, strlen(pDllName), MEM_COMMIT | MEM_RESERVE, PAGE_EXECUTE_READWRITE);
	
	// 将要注入的DLL路径写入到目标进程
	WriteProcessMemory(hProcess, lpParaAddress, pDllName, strlen(pDllName), 0);

	// 获取LoadLibrary的地址
	lpStartAddress = (LPTHREAD_START_ROUTINE)GetProcAddress(GetModuleHandle("kernel32"),"LoadLibraryA");

	// 创建远程线程执行注入DLL操作
	hRemoteThread = CreateRemoteThread(hProcess,NULL,0,lpStartAddress,lpParaAddress,0,NULL);	
	printf("%d\n", GetLastError());
	// 等待其加载完毕
	WaitForSingleObject(hRemoteThread,INFINITE);
	CloseHandle(hRemoteThread);

	system("pause");

	// 获取GetModuleHandleA的地址
	lpStartAddress = (LPTHREAD_START_ROUTINE)GetProcAddress(GetModuleHandle("kernel32"),"GetModuleHandleA");
	// 创建远程线程执行GetModuleHandleA操作
	hRemoteThread = CreateRemoteThread(hProcess,NULL,0,lpStartAddress,lpParaAddress,0,NULL);
	printf("%d\n", GetLastError());
	WaitForSingleObject(hRemoteThread,INFINITE); 
	GetExitCodeThread(hRemoteThread,&dwExitHandle);//线程的结束码即为Dll模块儿的句柄 
	CloseHandle(hRemoteThread); 

	// 获取FreeLibrary的地址
	lpStartAddress = (LPTHREAD_START_ROUTINE)GetProcAddress(GetModuleHandle("kernel32"),"FreeLibrary");

	// 创建远程线程执行卸载DLL操作
	hRemoteThread = CreateRemoteThread(hProcess,NULL,0,lpStartAddress,(LPVOID)dwExitHandle,0,NULL);

	// 等待其卸载完毕
	WaitForSingleObject(hRemoteThread,INFINITE);
	CloseHandle(hRemoteThread);

	// 释放资源
	VirtualFreeEx(hProcess,lpParaAddress,0,MEM_RELEASE);
	CloseHandle(hProcess);	

	system("pause");
	return 0;
}
```





#### OpenProcessToken

`OpenProcessToken` 是 Windows API 中的一个函数，用于打开与指定进程关联的访问令牌（Access Token）。以下是对该函数的详细描述：

```
#include <windows.h>

/**
 * @brief 打开指定进程的访问令牌
 *
 * 此函数用于打开与指定进程关联的访问令牌，以便后续对令牌的操作。
 *
 * @param hProcess 目标进程的句柄
 * @param dwDesiredAccess 指定访问令牌的访问权限
 * @param phToken 接收打开的访问令牌的句柄
 *
 * @return 如果函数成功，返回非零值；如果函数失败，返回零。要获取扩展的错误信息，可以调用 GetLastError 函数。
 *
 * @note 为了成功调用此函数，调用进程必须具有 PROCESS_QUERY_INFORMATION 或 PROCESS_QUERY_LIMITED_INFORMATION 权限。
 */
BOOL OpenProcessToken(
  HANDLE  ProcessHandle,          // 目标进程的句柄
  DWORD   DesiredAccess,         // 指定访问令牌的访问权限
  PHANDLE TokenHandle            // 接收打开的访问令牌的句柄
);


参数说明：

ProcessHandle：目标进程的句柄，该句柄必须具有 PROCESS_QUERY_INFORMATION 或 PROCESS_QUERY_LIMITED_INFORMATION 权限。
DesiredAccess：指定访问令牌的访问权限，可以使用常量如 TOKEN_READ，也可以通过按位 OR 运算组合多个权限。
TokenHandle：用于接收打开的访问令牌的句柄的指针。
返回值说明：

如果函数成功，返回非零值。
如果函数失败，返回零。要获取扩展的错误信息，可以调用 GetLastError 函数。

```





#### GetCurrentProcess

`GetCurrentProcess` 是 Windows 操作系统中的一个函数，通常在 Windows API 中使用。该函数用于获取当前执行代码的进程句柄（handle）。

```
#include <windows.h>

HANDLE GetCurrentProcess(void);

返回值：
返回一个 HANDLE 类型的值，代表当前进程的句柄。HANDLE 是一个通用的操作系统句柄类型，在 Windows 中用于表示各种内核对象，如进程、线程、文件等。在这种情况下，返回的 HANDLE 是当前进程的句柄。
```





#### LookupPrivilegeValue

`LookupPrivilegeValue` 是 Windows 操作系统中的一个函数，用于查找指定系统的特权（privilege）的本地名称和权限值。

```
#include <windows.h>

BOOL LookupPrivilegeValueA(
    LPCSTR lpSystemName,
    LPCSTR lpName,
    PLUID  lpLuid
);


参数：

lpSystemName：指向包含系统名的字符串的指针，一般为 NULL，表示本地计算机。
lpName：指向包含特权名的字符串的指针，例如 "SeShutdownPrivilege"。
lpLuid：指向 LUID 结构的指针，用于接收查找到的特权的本地唯一标识符。
返回值：
如果函数调用成功，返回非零值；如果函数调用失败，返回零。可以使用 GetLastError 函数获取更多错误信息。

```



#### CloseHandle

`CloseHandle` 是 Windows 操作系统中的一个函数，用于关闭一个打开的对象句柄。

```
#include <windows.h>

BOOL CloseHandle(
    HANDLE hObject
);


参数：

hObject：要关闭的对象句柄。
返回值：
如果函数调用成功，返回非零值；如果函数调用失败，返回零。可以使用 GetLastError 函数获取更多错误信息。
```



#### AdjustTokenPrivileges

`AdjustTokenPrivileges` 是 Windows 操作系统中的一个函数，用于修改访问令牌（access token）的权限。以下是其 C 语言描述：

```
#include <windows.h>

BOOL AdjustTokenPrivileges(
    HANDLE       TokenHandle,
    BOOL         DisableAllPrivileges,
    PTOKEN_PRIVILEGES NewState,
    DWORD        BufferLength,
    PTOKEN_PRIVILEGES PreviousState,
    PDWORD       ReturnLength
);


参数：
TokenHandle：要修改权限的令牌的句柄。
DisableAllPrivileges：指定是否禁用所有权限。如果为 TRUE，将禁用所有权限；如果为 FALSE，将启用或禁用由 NewState 参数指定的权限。
NewState：指向 TOKEN_PRIVILEGES 结构的指针，该结构包含了要设置的权限信息。
BufferLength：指定 NewState 参数指向的缓冲区的大小。
PreviousState：可选参数，指向 TOKEN_PRIVILEGES 结构的指针，用于接收修改之前的权限信息。
ReturnLength：可选参数，用于接收实际写入 PreviousState 缓冲区的字节数。

返回值：
如果函数调用成功，返回非零值；如果函数调用失败，返回零。可以使用 GetLastError 函数获取更多错误信息。
```



#### GetModuleFileName

`GetModuleFileName` 是 Windows 操作系统中的一个函数，用于获取指定模块的文件名。以下是其 C 语言描述：

```
#include <windows.h>

DWORD GetModuleFileNameA(
    HMODULE hModule,
    LPSTR   lpFilename,
    DWORD   nSize
);

参数：

hModule：指定要获取文件名的模块的句柄。如果为 NULL，表示获取当前执行代码的模块文件名。
lpFilename：指向一个缓冲区的指针，用于接收模块文件名的字符串。
nSize：指定缓冲区的大小，以字节为单位。

返回值：
如果函数调用成功，返回字符串的长度，不包括终止 null 字符；如果函数调用失败，返回零。可以使用 GetLastError 函数获取更多错误信息。
```



#### OpenProcess

`OpenProcess` 是 Windows 操作系统中的一个函数，用于打开一个已存在的进程，并返回该进程的句柄。以下是其 C 语言描述：

```
#include <windows.h>

HANDLE OpenProcess(
    DWORD dwDesiredAccess,
    BOOL bInheritHandle,
    DWORD dwProcessId
);

参数：
dwDesiredAccess：指定打开进程的访问权限。这是一个常量，如 PROCESS_ALL_ACCESS 表示完全访问权限。
bInheritHandle：指定返回的句柄是否可被子进程继承。一般设置为 FALSE。
dwProcessId：指定要打开的进程的标识符（ID）。

返回值：
如果函数调用成功，返回进程的句柄；如果函数调用失败，返回 NULL。可以使用 GetLastError 函数获取更多错误信息。
```



#### VirtualAllocEx

`VirtualAllocEx` 是 Windows 操作系统中的一个函数，用于在指定的远程进程中分配内存空间。以下是其 C 语言描述：

```
#include <windows.h>

LPVOID VirtualAllocEx(
    HANDLE hProcess,
    LPVOID lpAddress,
    SIZE_T dwSize,
    DWORD  flAllocationType,
    DWORD  flProtect
);

参数：
hProcess：要分配内存的目标进程的句柄。
lpAddress：指定分配内存的起始地址，如果为 NULL，由系统决定分配的地址。
dwSize：指定要分配的内存大小，以字节为单位。
flAllocationType：指定内存分配的类型，例如 MEM_COMMIT 表示提交内存，MEM_RESERVE 表示保留内存。
flProtect：指定内存区域的访问权限，例如 PAGE_READWRITE 表示可读写的内存。

返回值：
如果函数调用成功，返回分配的内存的起始地址；如果函数调用失败，返回 NULL。可以使用 GetLastError 函数获取更多错误信息。
```



#### WriteProcessMemory

`WriteProcessMemory` 是 Windows 操作系统中的一个函数，用于向指定的远程进程写入数据到指定的内存地址。以下是其 C 语言描述：

```
#include <windows.h>

BOOL WriteProcessMemory(
    HANDLE  hProcess,
    LPVOID  lpBaseAddress,
    LPCVOID lpBuffer,
    SIZE_T  nSize,
    SIZE_T  *lpNumberOfBytesWritten
);


参数：
hProcess：要写入数据的目标进程的句柄。
lpBaseAddress：指定要写入数据的远程进程的内存地址。
lpBuffer：指向包含要写入数据的缓冲区的指针。
nSize：指定要写入数据的字节数。
lpNumberOfBytesWritten：指向变量的指针，用于接收成功写入的字节数。

返回值：
如果函数调用成功，返回非零值；如果函数调用失败，返回零。可以使用 GetLastError 函数获取更多错误信息。
```





#### GetModuleHandle

`GetModuleHandle` 是 Windows 操作系统中的一个函数，用于获取指定模块的句柄。以下是其 C 语言描述：

```
#include <windows.h>

HMODULE GetModuleHandleA(
    LPCSTR lpModuleName
);

参数：
lpModuleName：指定要获取句柄的模块的名称。如果为 NULL，表示获取当前执行代码的模块句柄。

返回值：
如果函数调用成功，返回模块的句柄；如果函数调用失败，返回 NULL。可以使用 GetLastError 函数获取更多错误信息

```





#### GetProcAddress

`GetProcAddress` 是 Windows 操作系统中的一个函数，用于获取指定动态链接库（DLL）中导出函数或变量的地址。以下是其 C 语言描述：

```
#include <windows.h>

FARPROC GetProcAddress(
    HMODULE hModule,
    LPCSTR lpProcName
);

参数：
hModule：指定包含导出函数或变量的 DLL 的句柄。可以使用 LoadLibrary 或 GetModuleHandle 获取 DLL 的句柄。
lpProcName：指定要获取地址的导出函数或变量的名称。

返回值：
如果函数调用成功，返回导出函数或变量的地址；如果函数调用失败，返回 NULL。可以使用 GetLastError 函数获取更多错误信息。
```





#### CreateRemoteThread

`CreateRemoteThread` 是 Windows 操作系统中的一个函数，用于在远程进程中创建一个新的线程。以下是其 C 语言描述：

```
#include <windows.h>

HANDLE CreateRemoteThread(
    HANDLE hProcess,
    LPSECURITY_ATTRIBUTES lpThreadAttributes,
    SIZE_T dwStackSize,
    LPTHREAD_START_ROUTINE lpStartAddress,
    LPVOID lpParameter,
    DWORD dwCreationFlags,
    LPDWORD lpThreadId
);

参数：
hProcess：指定目标进程的句柄，新线程将在该进程中创建。
lpThreadAttributes：指定线程的安全性，通常设置为 NULL。
dwStackSize：指定新线程的堆栈大小，通常设置为 0 使用默认堆栈大小。
lpStartAddress：指定新线程的起始地址，即线程将从该地址开始执行。
lpParameter：传递给新线程的参数，可以为 NULL。
dwCreationFlags：指定线程的创建标志，通常设置为 0。
lpThreadId：指向变量的指针，用于接收新线程的标识符。

返回值：
如果函数调用成功，返回新线程的句柄；如果函数调用失败，返回 NULL。可以使用 GetLastError 函数获取更多错误信息。
```





#### GetLastError

`GetLastError` 是 Windows 操作系统中的一个函数，用于获取最近一次调用发生错误的错误代码。以下是其 C 语言描述：

```
#include <windows.h>

DWORD GetLastError(void);

返回值：
返回一个表示错误代码的 DWORD 类型的值。如果函数调用成功，返回的值通常是一个非零的错误代码。如果函数调用成功，返回的值通常是零。
```



#### WaitForSingleObject

`WaitForSingleObject` 是 Windows 操作系统中的一个函数，用于等待一个对象的状态变为 signaled。



```
#include <windows.h>

DWORD WaitForSingleObject(
    HANDLE hHandle,
    DWORD  dwMilliseconds
);

参数：

hHandle：要等待的对象的句柄。
dwMilliseconds：指定等待的时间，以毫秒为单位。如果为 INFINITE，则表示无限等待。
返回值：
如果函数调用成功，返回等待对象的状态。可能的返回值有：

WAIT_OBJECT_0：对象的状态变为 signaled。
WAIT_TIMEOUT：等待超时。
WAIT_FAILED：函数调用失败，可以使用 GetLastError 获取更多错误信息。
```



#### VirtualFreeEx

`VirtualFreeEx` 是 Windows 操作系统中的一个函数，用于释放在指定的远程进程中分配的虚拟内存空间。

```
#include <windows.h>

BOOL VirtualFreeEx(
    HANDLE hProcess,
    LPVOID lpAddress,
    SIZE_T dwSize,
    DWORD  dwFreeType
);
参数：
hProcess：目标进程的句柄，其中分配了虚拟内存空间。
lpAddress：要释放的虚拟内存的起始地址。
dwSize：要释放的虚拟内存的大小，以字节为单位。
dwFreeType：释放类型，可以是以下值之一：
MEM_DECOMMIT：释放虚拟内存的物理存储，但保留内存中的地址空间。
MEM_RELEASE：释放虚拟内存的物理存储和地址空间。

返回值：
如果函数调用成功，返回非零值；如果函数调用失败，返回零。可以使用 GetLastError 函数获取更多错误信息。
```





















































