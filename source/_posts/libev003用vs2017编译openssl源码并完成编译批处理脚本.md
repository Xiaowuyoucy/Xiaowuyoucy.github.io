---
title: 用vs2017编译openssl源码并完成编译批处理脚本
date: 2022-01-07 01:20:03
tags:
categories: libevent
doc:
---

## 编译openssl 

### 依赖项 

#### perl

`  https://pan.baidu.com/s/ 1BXPRcDAUFN2vJOrNvkZIwQ `

#### nasm 

默认安装目录: C:\Users\xiaca\AppData\Local\bin\NASM 

安装后配置到环境变量path中 

下载地址:`https://pan.baidu.com/s/1BXPRcDAUFN2vJOrNvkZIwQ `

#### 编译命令 

```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsamd64_x86.bat 
```

```
perl Confifigure { VC-WIN32 | VC-WIN64A | VC-WIN64I | VC-CE } --prefifix=%OUTPATH% 
perl Confifigure VC-WIN32 
```

nmake 

make test 

nmake install 

C:\Program Files (x86)\OpenSSL 

如果c盘需要管理员权限运行

### 步骤:

1.先安装perl和nasm

![image-20220107012532294](/images/javawz/image-20220107012532294.png)

全部点下一步



nasma解压后添加到PATH环境变量里

![image-20220107012731695](/images/javawz/image-20220107012731695.png)

控制台重启生效

<br /><br />

2.打开VS 2017的 x64_x86 交叉工具命令提示符

进入到openssl目录

输入`perl Configure VC-WIN32 --prefix=E:\libevent\out\vs2017_32\openssl`

`--prefix=` 指定输出路径

![image-20220107013355347](/images/javawz/image-20220107013355347.png)

<br /><br />

3.编译

```
nmake
```

![image-20220107013514294](/images/javawz/image-20220107013514294.png)

<br /><br />

4.安装

```
nmake install
```

![image-20220107013657853](/images/javawz/image-20220107013657853.png)

<br /><br />

## 编译批处理脚本

```
@echo "开始编译openssl"
set VS="C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsamd64_x86.bat"
set OUT=E:\libevent\out\vs2017_32\openssl
call %VS%
E:
cd E:\libevent\openssl-1.1.1
perl Configure VC-WIN32 --prefix=%OUT%
nmake clean
nmake
nmake install
@echo "build openssl end"
pause

```

