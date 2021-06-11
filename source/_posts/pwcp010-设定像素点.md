---
title: 设定像素点
date: 2021-06-05 13:59:49
tags:
categories: windows程序设计2
doc:
---

### COLORREF

颜色类型

实际上是 unsigned long

![1622873470897](/images/javawz/1622873470897.png)

### RGB宏

RGB(红,绿,蓝);

### SetPixel

 指定坐标处的像素设为指定的颜色 

```
 COLORREF SetPixel(HDC hdc, int X, int Y, COLORREF crColor)； 
```

```
SetPixel(hdc,i,j,color);
```

### GetPixel

```
COLORREF GetPixel(HDC hdc, int nXPos, int nYPos)
```

hdc：设备环境句柄。

nXPos：指定要检查的像素点的逻辑X轴坐标。

nYPos：指定要检查的像素点的逻辑Y轴坐标。

```
color = GetPixel(hdc,200,200);
```

### GetRValue宏

获取红色值

```
red = GetRValue(color);
```

### GetGValue宏

获取绿色值

```
green = GetGValue(color);
```

### GetBValue宏

获取蓝色值

```
blue = GetBValue(color);
```

