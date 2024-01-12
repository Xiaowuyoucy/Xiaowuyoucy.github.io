---
title: Win32基础
date: 2024-01-07 22:28:41
tags:
categories: 逆向工程
doc:
---

# 字符编码

### 标准的ASCII码

范围是0-127

![image-20240109230417590](/images/javawz/image-20240109230417590.png)





### ASCII码的拓展

范围是128-255

![image-20240109230502264](/images/javawz/image-20240109230502264.png)



### GB2312或GB2312-80

这两个编码是由两张ASCII码的拓展的编码组成

也就是说两个字节表示一个中文

字节1的范围是 128-255

字节2的范围是 128-255



### UNICODE编码



UNICODE编码包含全世界所有文字的一个编码表



Unicode编码范围是：0-0x10FFFF，可以容纳100多万个符号!

Unicode只是一个符号集，它只规定了符号的二进制代码，却没有规定这个二进制代码应该如何存储。





### 如何存储UNICODE

#### UTF-16

​	UTF-16编码以16位无符号整数为单位，注意是16位为一个单位，不表示一个字符就只有16位。这个要看字符的unicode编码处于什么范围而定，有可能是2个字节，也可能是4个字节现在机器上的unicode编码一般指的就是UTF-16

意思是utf-16是以两个字节来划分字符编码的,假如一个字符需要4个字节来表示,UTF-16形式存储会将4字节中的前两个字节编码和后两个字节编码分别存放.



#### UTF-8：

```
Unicode编码(16进制)　	║　UTF-8 字节流(二进制) 　
000000 - 00007F　	║　0xxxxxxx 　　
000080 - 0007FF　	║　110xxxxx 10xxxxxx 　　
000800 - 00FFFF　	║　1110xxxx 10xxxxxx 10xxxxxx 　　
010000 - 10FFFF　	║　11110xxx 10xxxxxx 10xxxxxx 10xxxxxx 

```

000000 - 00007F　范围用1个字节存放

000080 - 0007FF   范围用2个字节存放

000800 - 00FFFF	范围用3个字节存放

010000 - 10FFFF　范围用4个字节存放



#### BOM(Byte Order Mark)：

UTF-8 编码存放的文件十六进制开头是EF BB BF 　

UTF-16LE  	UTF-16小端存放开头是　	FF FE 　　
UTF-16BE  	UTF-16大端存放开头　	FE FF 　

```
UTF-8　    	║　	EF BB BF 　　
UTF-16LE  	║　	FF FE 　　
UTF-16BE  	║　	FE FF 　
```





# C语言中的宽字符

```
char szStr[] = "中国";

wchar_t swzStr[] = L"中国";

```

如果字符串前面不加L,默认是以IDE项目设置的字符集形式，可能是ASCII码

字符串前面加上L，表示用UNICODE字符集存放

wchar_t是宽字符类型，两个字节



### 打印宽字符

```
#include <locale.h>		//要包含这个头文件

setlocale(LC_ALL,"");//使用控制台默认的编码

wchar_t swzStr[] = L“中国”;

wprintf(L"%s\n",x1);

```



### 字符串长度

```
char szStr[] = "中国";								
wchar_t swzStr[] = L"中国";								
strlen(szStr);	//取得多字节字符串中字符长度，不包含 00										
wcslen(swzStr);	//取得多字节字符串中字符长度，不包含 00 00
```



### 常用函数

```
char		wchar_t		//多字节字符类型   宽字符类型									
printf		wprintf		//打印到控制台函数									
strlen		wcslen		//获取长度			
							
strcpy		wcscpy		//字符串复制			
							
strcat		wcscat		//字符串拼接			
							
strcmp		wcscmp		//字符串比较			
							
strstr		wcsstr		//字符串查找	

```





# Win32  API中的宽字符

### 什么是Win32 API? 有哪些? 在哪里? 

主要是存放在 C:\WINDOWS\system32 下面所有的dll

如果是64位系统，system32存放的是64位的DLL，SysWOW64存放的是32位的DLL



几个重要的DLL：

<1> Kernel32.dll：最核心的功能模块，比如管理内存、进程和线程相关的函数等。

<2> User32.dll：是Windows用户界面相关应用程序接口,如创建窗口和发送消息等。

<3> GDI32.dll：全称是Graphical Device Interface(图形设备接口),包含用于画图和显示文本的函数。



### 常用数据类型：

```
汇编：
	byte 		BYTE		PBYTE
	word		WORD		PWORD
	dword		DWORD		PDWORD
	
C语言：
	char				CHAR			PCHAR
	unsigned char		UCHAR			PUCHAR
	short				SHORT			PSHORT
	unsigned short		USHORT			PUSHORT
	int					INT				PINT
	unsigned int		UINT			PUINT  
	
C++语言：
	bool		BOOL

```





### 在Win32中使用字符串：

#### 字符类型：

最好使用TCHAR类型,因为TCHAR类型会根据IDE设置的字符集来选择用哪个类型，如果IDE使用的是ASCII字符集,选择的就是CHAR类型，否则选择的是WCHAR类型。



```
CHAR  szStr[] = “中国”;	
WCHAR  swzStr[] = L“中国”;
TCHAR stzSr[] = TEXT(“中国”);
```

 TEXT(“中国”)，会自动将字符串转换为当前IDE设置的字符集

#### 字符串指针：

PTSTR也是会根据IDE设置的字符集来选择什么类型的字符串指针

```
PSTR  pszStr = “中国”;	
PWSTR pwszStr = L“中国”;
PTSTR ptszStr = TEXT(“中国”);
```





### 第一个Win32 API的使用：

写Win32程序要包含windows.h头文件

```
CHAR szTitle[] = "标题";
CHAR szContent[] = "欢迎大家来的Win32 API世界!";
MessageBoxA(0,szContent,szTitle,MB_OK);

```



```
WCHAR swzTitle[] = L"标题";
WCHAR swzContent[] = L"欢迎大家来的Win32 API世界!";
MessageBoxW(0,swzContent,swzTitle,MB_OK);

```



```
TCHAR stzTitle[] = TEXT("标题");
TCHAR stzContent[] = TEXT("欢迎大家来的Win32 API世界!");
MessageBox(0,stzContent,stzTitle,MB_OK);

```

MessageBox 会根据平台使用的字符集来选择调用MessageBoxA还是MessageBoxW，其实MessageBoxA内部也是调用MessageBoxW的，只不过会将字符串参数转换为Unicode编码，然后再调用





# 进程



### 什么是进程

进程提供程序所需的资源，如：数据、代码等等。



### 进程内存空间的地址划分

| **分区**     | **x86 32位Windows**     |
| ------------ | ----------------------- |
| 空指针赋值区 | 0x00000000 - 0x0000FFFF |
| 用户模式区   | 0x00010000 - 0x7FFEFFFF |
| 64KB禁入区   | 0x7FFF0000 - 0x7FFFFFFF |
| 内核         | 0x80000000 - 0xFFFFFFFF |



### 进程的组成部分：模块

在OD中查看进程所包含的模块。

每个模块都是一个可执行文件，遵守相同的格式，即PE结构。



### 进程的创建： 

 任何进程都是别的进程创建的：CreateProcess()

进程的创建过程
	1、映射EXE文件
	2、创建内核对象EPROCESS
	3、映射系统DLL(ntdll.dll)
	4、创建线程内核对象ETHREAD
	5、系统启动线程
		映射DLL(ntdll.LdrInitializeThunk)
		线程开始执行



![image-20240110222553959](/images/javawz/image-20240110222553959.png)



![image-20240110222608956](/images/javawz/image-20240110222608956.png)

![image-20240110222631845](/images/javawz/image-20240110222631845.png)



![image-20240110222817938](/images/javawz/image-20240110222817938.png)



### 创建进程

#### CreateProcess

```
BOOL CreateProcess(
  LPCTSTR               lpApplicationName,        // 应用程序的可执行文件名或路径
  LPTSTR                lpCommandLine,            // 命令行参数
  LPSECURITY_ATTRIBUTES lpProcessAttributes,      // 进程对象的安全属性
  LPSECURITY_ATTRIBUTES lpThreadAttributes,       // 主线程对象的安全属性
  BOOL                  bInheritHandles,          // 指定新进程是否可以继承当前进程的句柄
  DWORD                 dwCreationFlags,          // 控制进程的创建标志
  LPVOID                lpEnvironment,            // 指向新进程的环境块的指针
  LPCTSTR               lpCurrentDirectory,       // 指定新进程的当前工作目录
  LPSTARTUPINFO         lpStartupInfo,            // 指定主窗口的外观及默认的I/O属性
  LPPROCESS_INFORMATION lpProcessInformation      // 接收新进程和其主线程的信息
);

```



```c
#include <windows.h>
#include <tchar.h>

int _tmain(int argc, _TCHAR* argv[])
{
    // 指定要执行的命令
    LPCTSTR command = _T("C:\\路径\\到\\你的\\可执行文件.exe");

    // 创建进程信息结构
    PROCESS_INFORMATION processInfo;
    ZeroMemory(&processInfo, sizeof(PROCESS_INFORMATION));

    // 创建启动信息结构
    STARTUPINFO startupInfo;
    ZeroMemory(&startupInfo, sizeof(STARTUPINFO));
    startupInfo.cb = sizeof(STARTUPINFO);

    // 创建进程
    if (CreateProcess(
        NULL,               // 没有模块名（使用命令行）
        command,            // 命令行
        NULL,               // 进程句柄不可继承
        NULL,               // 线程句柄不可继承
        FALSE,              // 不继承句柄
        0,                  // 无特殊标志
        NULL,               // 使用父进程环境
        NULL,               // 使用父进程目录
        &startupInfo,       // 启动信息结构
        &processInfo        // 进程信息结构
    ))
    {
        // 等待子进程退出
        WaitForSingleObject(processInfo.hProcess, INFINITE);

        // 关闭进程和线程句柄
        CloseHandle(processInfo.hProcess);
        CloseHandle(processInfo.hThread);
    }
    else
    {
        _tprintf(_T("无法创建进程，错误码：%d\n"), GetLastError());
    }

    return 0;
}

```



#### STARTUPINFO

```
typedef struct _STARTUPINFO {
  DWORD  cb;                            // 结构体的大小，用于指定结构体版本
  LPTSTR lpReserved;                    // 保留，必须为NULL
  LPTSTR lpDesktop;                     // 指定新进程的桌面，通常为NULL
  LPTSTR lpTitle;                       // 指定新进程的控制台窗口标题，通常为NULL
  DWORD  dwX;                           // 指定新进程窗口的初始X坐标
  DWORD  dwY;                           // 指定新进程窗口的初始Y坐标
  DWORD  dwXSize;                       // 指定新进程窗口的初始宽度
  DWORD  dwYSize;                       // 指定新进程窗口的初始高度
  DWORD  dwXCountChars;                 // 指定新进程窗口的初始宽度（字符单位）
  DWORD  dwYCountChars;                 // 指定新进程窗口的初始高度（字符单位）
  DWORD  dwFillAttribute;               // 控制新进程窗口的文本和背景颜色
  DWORD  dwFlags;                       // STARTF_* 标志位，用于指定 STARTUPINFO 结构体的标志
  WORD   wShowWindow;                   // 指定新进程窗口的显示状态
  WORD   cbReserved2;                   // 保留，必须为0
  LPBYTE lpReserved2;                   // 保留，必须为NULL
  HANDLE hStdInput;                     // 指定新进程的标准输入句柄
  HANDLE hStdOutput;                    // 指定新进程的标准输出句柄
  HANDLE hStdError;                     // 指定新进程的标准错误句柄
} STARTUPINFO, *LPSTARTUPINFO;

```



#### PROCESS_INFORMATION

typedef struct _PROCESS_INFORMATION {
  HANDLE hProcess;           // 新进程的句柄，用于操作新进程
  HANDLE hThread;            // 新进程的主线程的句柄，用于操作新进程的主线程
  DWORD  dwProcessId;        // 新进程的进程标识符
  DWORD  dwThreadId;         // 新进程的主线程标识符
} PROCESS_INFORMATION, *LPPROCESS_INFORMATION;



#### GetLastError

- `GetLastError` 函数返回一个 `DWORD` 类型的错误代码，表示最近一次发生的错误。

通常，当一个 Windows API 函数调用失败时，可以使用 `GetLastError` 函数获取详细的错误信息。错误代码可以通过查阅 Windows API 文档或使用 `FormatMessage` 函数转换为可读的错误消息。

```
DWORD GetLastError(void);
```



#### ZeroMemory 

`ZeroMemory` 不是一个单独的函数，而是一个宏（macro），通常用于将内存区域的内容全部设置为零。以下是 `ZeroMemory` 宏的定义和简要中文注释：

```
#define ZeroMemory(Destination, Length) memset((Destination), 0, (Length))

```

- `ZeroMemory` 宏使用 `memset` 函数将目标内存区域的内容全部设置为零。
- `Destination`：指向要清零的内存区域的指针。
- `Length`：要清零的内存区域的字节数。

这个宏的作用等同于使用 `memset` 函数将内存清零，但它是一种简化的写法。在实际使用中，可以选择使用 `memset` 函数或 `ZeroMemory` 宏，两者的效果是相同的。



#### GetStartupInfo

`GetStartupInfo` 是一个用于获取当前进程的 `STARTUPINFO` 结构体信息的 Windows API 函数。以下是该函数的原型和简要中文注释：

```
void GetStartupInfo(
  LPSTARTUPINFO lpStartupInfo  // 指向 STARTUPINFO 结构体的指针，用于接收当前进程的启动信息
);

```



调试器填写的STARTUPINFO 成员和系统打开进程填写的成员,内容是不一样的



#### CloseHandle

`CloseHandle` 是一个 Windows API 函数，用于关闭一个打开的内核对象的句柄。以下是该函数的原型和简要中文注释：

```
BOOL CloseHandle(
  HANDLE hObject  // 要关闭的内核对象的句柄
);
```

