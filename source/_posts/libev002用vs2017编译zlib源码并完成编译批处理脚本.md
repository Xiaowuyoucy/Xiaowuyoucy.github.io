---
title: 用vs2017编译zlib源码并完成编译批处理脚本
date: 2022-01-07 00:44:23
tags:
categories: libevent
doc:
---

##  编译zlib 

### 编译32位 

```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsamd64_x86.bat 
```

<br /><br />

### 编译64位 

```
"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvars64.bat" 
```

```
nmake -f win32/Makefifile.msc
```



<br /><br />

### VS 2017的 x64_x86 交叉工具命令提示符

![image-20220107005147178](/images/javawz/image-20220107005147178.png)

64位平台编译32位用这个工具

<br /><br />

### 适用于 VS 2017 的 x64 本机工具命令提示

编译64位源码用这个工具

![image-20220107005205464](/images/javawz/image-20220107005205464.png)

<br /><br />

### 适用于 VS 2017 的 x86_x64 兼容工具命令提示

用32位平台来编译64位源码

![image-20220107005440939](/images/javawz/image-20220107005440939.png)

<br /><br />

#### 打开VS 2017的 x64_x86 交叉工具命令提示符并且进入到zlib目录

![image-20220107005557569](/images/javawz/image-20220107005557569.png)

### 编译

```
nmake /f WIN32\Makefile.msc
```

![image-20220107005748153](/images/javawz/image-20220107005748153.png)

<br /><br />

## 编译批处理脚本

```
@echo "开始编译zlib"
set VS="D:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsamd64_x86.bat"
set OUT=F:\libevent\out\vs2017_32\zlib
call %VS%
cd zlib-1.2.11
nmake /f win32\Makefile.msc clean
nmake /f win32\Makefile.msc
md %OUT%\lib
md %OUT%\bin
md %OUT%\include
copy /Y *.lib %OUT%\lib
copy /Y *.h %OUT%\include
copy /Y *.dll %OUT%\bin
copy /Y *.exe %OUT%\bin
@echo "zlib编译结束"
pause

```

<br />

```
set VS=				//自己VS 2017的 x64_x86 交叉工具命令提示符的路径
```

<br />

```
set OUT=			//输出文件的路径
```

<br />

```
call %VS%			//代表调用vcvarsamd64_x86.bat
```

<br />

```
nmake /f win32\Makefile.msc clean		//清理编译出来的文件
```

<br />

```
nmake /f win32\Makefile.msc			//编译
```

<br />

```
md %OUT%\lib						//创建lib目录
md %OUT%\bin						//创建bin目录
md %OUT%\include					//创建include目录
```

<br />

```
copy /Y *.lib %OUT%\lib				//不提示复制所有后缀为.lib的文件到%OUT%\lib目录
copy /Y *.h %OUT%\include			//不提示复制所有后缀为.h的文件到%OUT%\include目录
copy /Y *.dll %OUT%\bin				//不提示复制所有后缀为.dll的文件到%OUT%\bin目录
copy /Y *.exe %OUT%\bin				//不提示复制所有后缀为.exe的文件到%OUT%\bin目录
```

