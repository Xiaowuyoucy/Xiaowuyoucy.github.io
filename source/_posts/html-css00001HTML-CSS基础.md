---
title: HTML/CSS基础
date: 2020-02-12 21:45:47
tags:
categories: HTML/CSS
doc:
---

# HTML/CSS基础

## 软件架构

### `C/S`,客户端/服务器端

&emsp;&emsp;1.一般我们使用的软件都是`C/S`架构

&emsp;&emsp;2.比如系统中的软件QQ,360,office等等

&emsp;&emsp;3.`C`表示客户端,用户通过客户端来使用软件

&emsp;&emsp;4.`S`表示服务器,服务器负责处理软件的业务逻辑

#### &emsp;&emsp;特点:

&emsp;&emsp;&emsp;1.软件使用前必须得安装

&emsp;&emsp;&emsp;2.软件更新时,服务器和客户端得同时更新

&emsp;&emsp;&emsp;3.`C/S`架构的软件不能跨平台使用

&emsp;&emsp;&emsp;4.`C/S`架构的软件客户端和服务器通信采用的是自有协议,相对来说比较安全

### B/S,浏览器/服务器

&emsp;&emsp;1.`B/S`本质上也是`C/S`,只不过`B/S`架构的软件,使用浏览器作为软件客户端

&emsp;&emsp;2.`B/S`架构软件通过使用浏览器访问网页的形式,来使用软件,比如:京东,淘宝,知乎等等

#### &emsp;&emsp;特点:

&emsp;&emsp;&emsp;1.软件不需要安装,直接使用浏览器访问指定的网址即可

&emsp;&emsp;&emsp;2.软件更新时,客户端不需要更新

&emsp;&emsp;&emsp;3.软件可以跨平台,只要系统中有浏览器,就可以使用.

&emsp;&emsp;&emsp;4.B/S架构的软件,客户端和服务器之间通信采用的是通用的`HTTP`协议,相对来说不安全,一般采取`https`协议



## W3C标准:

一个网页主要由三部分组成:`结构`,`表现`,`行为`.

结构:对应的是`html`  ,用于描述页面的结构

表现:对应的是`CSS`, 用于控制页面中元素的样式

行为:对应的是`JavaScript` , 用于响应用户操作



## HTML简介

`HTML`(Hypertext Markup Language)

`超文本标记语言`

负责网页的三个要素中的`结构`

HTML使用`标签`的形式来标识网页中的不同组成部分

所谓超文本指的是`超链接`,使用超链接可以让我们从一个页面跳转到另一个页面.

扩展名: ` .html`

标准格式:

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	
</body>
</html>
```

