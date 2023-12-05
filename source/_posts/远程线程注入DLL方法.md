---
title: 远程线程注入DLL方法
date: 2023-12-05 00:43:56
tags:
categories:
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

