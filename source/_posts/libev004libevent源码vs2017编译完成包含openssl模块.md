---
title: libevent源码vs2017编译完成包含openssl模块
date: 2022-01-07 01:46:28
tags:
categories: libevent
doc:
---

## 编译libevent 

### 编译命令 

```
C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsamd64_x86.bat 
```

```
nmake /f Makefifile.nmake clean 
```

```
nmake /f Makefifile.nmake OPENSSL_DIR=D:\lib\openssl 
```

`OPENSSL_DIR=`  指定openssl目录

### 出现问题 

#### openssl问题 

修改test目录下 Makefifile.nmake 文件内容

```
SSL_LIBS=..\libevent_openssl.lib $(OPENSSL_DIR)\lib\libeay32.lib $(OPENSSL_DIR)\lib\ssleay32.lib gdi32.lib User32.lib 

```

改成

```
SSL_LIBS=..\libevent_openssl.lib $(OPENSSL_DIR)\lib\libssl.lib $(OPENSSL_DIR)\lib\libcrypto.lib gdi32.lib User32.lib
```

<br />

<br />

### 步骤:

1.打开VS 2017的 x64_x86 交叉工具命令提示符<br /><br />

2.进入到`libevent-master`目录

![image-20220107020711140](/images/javawz/image-20220107020711140.png)<br /><br />

3.输入`nmake /f Makefifile.nmake OPENSSL_DIR=D:\lib\openssl `

` OPENSSL_DIR=` 这里的路径是我们生成的openssl路径

![image-20220107020837005](/images/javawz/image-20220107020837005.png)<br />

<br />

4.出现错误

![image-20220107020942573](/images/javawz/image-20220107020942573.png)

找不到libeay32.lib

可以改掉自己的openssl文件

或者改掉libevent-master\test\Makefile.nmake文件内容<br /><br />

5.打开regress.exe程序

可能会出现缺失libcrypto-1_1.dll文件和libssl-1_1.dll文件

下载回来之后放在`C:\Windows\SysWOW64`目录即可