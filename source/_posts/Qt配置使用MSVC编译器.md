---
title: Qt配置使用MSVC编译器
date: 2022-06-16 00:08:20
tags:
categories: Qt
doc:
---

 **注意:Qt支持的MSVC就是2017和2015，所以vs也要下载2017，不要直接用最新的，安装路径都用默认的。程序运行失败时可以尝试windeployqt拷贝库文件到本地，然后有可能就能运行了**

[VS官网下载](https://visualstudio.microsoft.com/zh-hans/downloads/)Visual Studio Community 2017，在线安装Visual Studio Community2017版本。vs2017如何配置见附文一

![在这里插入图片描述](/images/javawz/c75aa6beb6a0fbcc0cdb756a5c963b56.png)

[下载并安装Windows10 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk/),

![在这里插入图片描述](/images/javawz/abe739fb9bb25bc7581af57e39983b6d.png)

直接下载.exe版本，然后安装。安装中，切记选择Debugging Toolsfor Windows组件，因为在后面需要用这个。



下载并安装Qt Creator，由于最新的版本好像没有集成mysql的驱动，所以我使用的是较先的5.12.2版本。安装Qt Creator的过程中，记得在安装组件过程中，安装MSVC2017 32位和MSVC2017 32位。注意：建议32位和64位都安装，32位可以在64位系统使用，但是返过来不行

![img](/images/javawz/45292b5514941d8d03ff3e3693e71dd8.png)

配置Debuggers，具体位置在菜单栏-工具-选项-Kits-Debuggers中。当你Windows 10 SDK安装成功后，他会自动出现这两个exe。

![在这里插入图片描述](/images/javawz/78ad27fd2b2e331033695046123fc873.png)



当然你也可以自己配置一个，具体如下：

![在这里插入图片描述](/images/javawz/98a40306a1bc7e3f2972f0c30bc4a007.png)

我配置的就是这样：

![在这里插入图片描述](/images/javawz/713987ea46b5f34ddc93f68c46cb4023.png)

配置构建套件（Kit），具体位置在菜单栏-工具-选项-Kits-构建套件（Kit）中。之前没有SDK或者其他原因，你的套件前面就会有一个黄色的感叹号。这种你就可以自己更改他的设置：

![在这里插入图片描述](/images/javawz/b5e4633ca4da9dcf35c1c0dce2a874e6.png)

或者你可以自己新建一个，具体配置如下：

![在这里插入图片描述](/images/javawz/f37ceb0f6a279408a04134683423c2c3.png)



### 注意：这里MSVC的kit需要你自己配置，不配是不会自动识别的





**附文一：安装VS2017并配置QT**

1. 打开VS2017，菜单栏 工具-> 扩展和更新，点击 联机 然后搜索qt，下载

![img](/images/javawz/fda53c8963e6fa4437387e3147c0d9a3.png)

2. 下载完成后，关闭VS2017，出现以下安装画面，在其初始化完成后点击“修改”，等待配置完成即可

![img](/images/javawz/4df9703ba669dcfc0817b999079ae264.png)

3. 再次打开VS 2017，菜单栏出现 Qt VS tools，新建项目也出现QT项目选项

![img](/images/javawz/4359d919b09e09cb7a0794c887855c06.png)

![img](/images/javawz/bc1a2a8ea8d130616bee43c22eed8928.png)

底部输出框出现：

![img](/images/javawz/c034a2679bc0525ce416c9a74dbe4b92.png)

4. 菜单栏 -> Qt Vs tools -> Qt options -> Add，添加如下路径

![img](/images/javawz/c8f3a93e1ee0045be9f5ca7603693e5f.png)

5. 环境变量配置： 这台电脑 -> 属性 -> 高级系统设置 -> 环境变量 ，添加Path变量：

![img](/images/javawz/ef4711f4bd73c576c1df9170f475f819.png)

注意：将C:\Program Files(x86)\Windows Kits\10\bin\10.0.17134.0\x64添加到系统环境变量的Path中去。

​     Win10 系统环境变量修改方法：此电脑——属性——高级系统设置——高级——环境变量——系统变量——Path——编辑。将路径复制进去后，点击保存便可。



6. 安装配置完成

