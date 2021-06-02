---
title: 通盘考虑
date: 2021-05-31 17:45:08
tags:
categories: windows程序设计2
doc:
---

# 通盘考虑

Loadicon 加载图标，以供程序使用。

LoadCursor加载鼠标光标，以供程序使用。

GetStockObject 获取一个图形对象。在本例中是一个用来对窗口的背景进行重绘
的画刷。

RegisterClass 为应用程序的窗口注册一个窗口类。

MessageBox 显示消息框。

Create Window 基于窗口类创建一个窗口。

ShowWindow 在屏幕中显示窗口。

UpdateWindow 指示窗口对其自身进行重绘。

GetMessage 从消息队列获取消息。

TranslateMessage 翻译一些键盘消息。

DispatchMessage 将消息发送给窗口过程。

PlaySound 播放声音文件。

BeginPaint 标明窗口绘制开始。

GetClientRect 获取窗口客户区的尺寸。

DrawText 显示一个文本字符串。

EndPaint结束窗口绘制。

PostQuitMessage 将“退出”消息插入消息队列。

DefWindowProc 执行默认的消息处理。



### 大写标识符

![1622454625727](/images/javawz/1622454625727.png)



### 匈牙利标记法

![1622454896616](/images/javawz/1622454896616.png)



### GetStockobject

 获取预定义的备用笔、刷子、字体或者调色板的句柄。 

```c
HGDIOBJ GetStockObject(int fnObject)；
```

fnObject：指定对象的类型，该参数可取如下值之一；

BLACK_BRUSH：黑色画刷；DKGRAY_BRUSH：暗灰色画刷；

DC_BRUSH：在[Windows98](https://baike.baidu.com/item/Windows98/5246326),Windows NT 5.0和以后版本中为纯颜色画刷，缺省色为白色，可以用[SetDCBrushColor](https://baike.baidu.com/item/SetDCBrushColor)函数改变颜色，更多的信息参见以下的注释部分。

GRAY_BRUSH：灰色画刷笔；

HOLLOW_BRUSH：空画刷（相当于NULL_BRUSH）；

NULL_BRUSH：空画刷（相当于HOLLOW_BRUSH）；

LTGRAY_BRUSH：亮灰色画刷；

WHITE_BRUSH：白色画刷；

BLACK_PEN：黑色钢笔；

DC_PEN：在[Windows98](https://baike.baidu.com/item/Windows98/5246326)、Windows NT 5.0和以后版本中为纯色钢笔，缺省色为白色，使用[SetDCPenColor](https://baike.baidu.com/item/SetDCPenColor)函数可以改变色彩，更多的信息，参见下面的注释部分。

WHITE_PEN：白色钢笔；

ANSI_FIXED_FONT：在Windows中为固定间距（等宽）系统字体；

ANSI_VAR_FONT：在Windows中为变间距（比例间距）系统字体；

DEVICE_DEFAUCT_FONT：在WindowsNT中为设备相关字体；

DEFAULT_GUI_FONT：用户界面对象缺省字体，如菜单和对话框；

OEM_FIXED_FONT：[原始设备制造商](https://baike.baidu.com/item/原始设备制造商)（OEM）相关固定间距（等宽）字体；

SYSTEM_FONT：系统字体，在缺省情况下，系统使用系统字体绘制菜单，对话框控制和文本；

SYSTEM_FIXED_FONT：固定间距（等宽）系统字体，该对象仅提供给兼容16位Windows版本；

DEFAULT_PALETTE：缺省[调色板](https://baike.baidu.com/item/调色板)，该调色板由系统调色板中的静态色彩组成。