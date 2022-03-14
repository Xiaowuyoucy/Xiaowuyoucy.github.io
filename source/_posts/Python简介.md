---
title: Python简介
date: 2022-03-03 17:47:53
tags:
categories: Python
doc:
---

## Python介绍

1. 简单、易学，适应人群广泛
2. 免费、开源
3. 应用领域广泛



# Python版本

- Python 2.X
- Python 3.X
  - Python 3.5
  - Python 3.6
  - Python 3.7 



# 解释器的作用

Python解释器作用：运行文件

- Python解释器种类
  - CPython，C语言开发的解释器[官方]，应用广泛的解释器。
  - IPython，基于CPython的一种交互式解释器。
  - 其他解释器
    - PyPy，基于Python语言开发的解释器。
    - Jython，运行在Java平台的解释器，直接把Python代码编译成Java字节码执行。
    - IronPython，运行在微软.Net平台上的Python解释器，可以直接把Python代码编译成.Net的字节码。

# 下载Python解释器

下载地址：https://www.python.org/downloads/release/python-372/

[单击上述链接] -- 查找目标文件：Windows x86-64 executable installer -- 单击即可下载。

![image-20190110170855787-7111335](/images/javawz/image-20190110170855787-7111335.png)



# 安装Python解释器

双击可执行文件 — 勾选[pip] -- [Next] -- [勾选添加环境变量] -- [Install]，按提示操作即可。

![image-20190122112451519](/images/javawz/image-20190122112451519.png)



## PyCharm

- PyCharm的作用
- 下载安装PyCharm
- PyCharm的基本使用
- PyCharm的基本设置

## PyCharm的作用

PyCharm是一种Python ==IDE==（集成开发环境），带有一整套可以帮助用户在使用Python语言开发时==提高其效率的工具==，内部集成的功能如下：

- Project管理
- 智能提示
- 语法高亮
- 代码跳转
- 调试代码
- 解释代码(解释器)
- 框架和库
- ......

> PythonCharm分为专业版（professional）和社区版（community），本视频以社区版为基准进行操作和讲解。

# 下载和安装

## 2.1 下载

下载地址：http://www.jetbrains.com/pycharm/download/#section=windows

![image-20190110172355909](/images/javawz/image-20190110172355909.png)

注意：这里选择Community(社区版)下载，专业版是收费版本，社区版可以满足我们基础编程需求。

## 2.2 安装

双击安装包 -- [运行] -- [允许你应用更改设备]: [是] -- [Next] -- [选择安装位置] -- [Next] -- [Install] -- [Finish]。

![image-20190108112855414-6918135](/images/javawz/image-20190108112855414-6918135.png)

# PyCharm基本使用

## 3.1 新建项目

打开PyCharm -- [Create New Project] -- 选择项目根目录和解释器版本 -- [Create]，即可完成新建一个项目。

![image-20190102112309128-6399389](/images/javawz/image-20190102112309128-6399389-1646321297852.png)

## 3.2 新建文件并书写代码

项目根目录或根目录内部任意位置 — 右键 -- [New] -- [Python File] -- 输入文件名 -- [OK]

> 如果是将来要上传到服务器的文件，那么文件名切记不能用中文。

![image-20190102114520725-6400720](/images/javawz/image-20190102114520725-6400720.png)

双击打开文件，并书写一个最简单的Python代码：

``` python
print("hello world")
```

## 3.3 运行文件

文件打开状态 -- 空白位置 — 右键 -- Run -- 即可调出Pycharm的控制台输出程序结果。

![image-20190102122728572-6403248](/images/javawz/image-20190102122728572-6403248.png)

## 四. PyCharm的基本设置

[file] -- [Settings]/[Default Settings]。

## 4.1 修改主题

[Appearance & Behavior] -- [Appearance]

- Theme：修改主题
- Name：修改主题字体
- Size：修改主题字号

## 4.2 修改代码文字格式

[Editor] -- [Font]

- Font：修改字体
- Size：修改字号
- Line Spacing：修改行间距

## 4.3 修改解释器

[Project: 项目名称] -- [Project Interpreter] -- [设置图标] -- [Add] -- 浏览到目标解释器 -- [OK] -- [OK]。

## 4.4 项目管理

### 4.4.1 打开项目

[File] -- [Open] -- 浏览选择目标项目根目录 -- [OK] -- 选择打开项目方式。

打开项目的方式共三种，分别如下

![image-20190115095119315](/images/javawz/image-20190115095119315.png)

1. This Window 

覆盖当前项目，从而打开目标项目

2. New Window

在新窗口打开，则打开两次PyCharm，每个PyCharm负责一个项目。

3. Attach

在当前项目中打开,且不覆盖

### 4.4.2 关闭项目

[File] -- [Close Project]/[Close Projects in current window]

# 总结

- PyCharm新建文件

项目管理空白位置 — 右键 -- New -- PythonFile

- 运行文件

代码内部 — 右键 -- Run

- 修改代码文字格式

[file] -- [Settings]/[Default Settings] -- [Editor] -- [Font]