---
title: xHtml的语法规范
date: 2020-02-18 05:25:32
tags:
categories: HTML/CSS
doc:
---

# xHtml的语法规范

##### 浏览器会尽最大的努力正确解析错误的语法,你所有的不符合语法规范的内容,浏览器都会为你自动修正,但是有些情况会修正错误.

<br />

<br />

1.HTML中不区分大小写,但是我们一般都使用小写

```html
<p>aaaaaaaaa</p>
```

2.HTML中的注释不能嵌套

```html
<--正确写法-->

=================================================
错误写法
<--

	<--

	-->

-->
```

3.HTML标签必须结构完整,要么成对出现,要么自结束标签.

```html
<--正确写法-->
<p>aaaaa</p> 

<--错误写法-->
<p>asdfdsf
```

4.HTML标签可以嵌套,但是不能交叉嵌套.

```html
<--正确写法-->
<p>aaaa<font color="red;">aaa</font>aaa</p>

<--错误写法-->
<p>aaaa<font color="red;">aaaaaa</p></font>

```

5.HTML标签中的属性必须有值,且值必须加引号(双引号,单引号都可以)

```html
<--正确写法-->
<p>aaaa<font color="red;">aaa</font>aaa</p>

<--错误写法-->
<p>aaaa<font color>aaa</font>aaa</p>
```

