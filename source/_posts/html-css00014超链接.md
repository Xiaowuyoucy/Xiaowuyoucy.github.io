---
title: 超链接
date: 2020-02-18 09:46:04
tags:
categories: HTML/CSS
doc:
---

# 超链接

使用超链接可以让我们从一个页面跳转到另一个页面

##### 使用a标签来创建一个超链接:

##### 属性:

`href`:指向链接跳转的目标地址,可以写一个相对路径也可以写一个完整地址.

`target`:可以用来指定打开链接的位置

&emsp;可选值:

&emsp;&emsp;&emsp;`_self`:表示在当前窗口中打开(默认值)

&emsp;&emsp;&emsp;`_blank`:在新的窗口中打开链接

​		<br/>

<br/>

```html
<!-- 超链接 -->
<a href="http://www.baidu.com">我是一个超链接</a>
<a href="demo.html"> 我是一个超链接</a>
```

<br/>

<br/>

<br/>

在新窗口打开页面

```html
<!-- 在本窗口跳转 -->
<a href="http://www.baidu.com" target="_self">我是一个超链接</a>
<-- 在新窗口打开 -->
<a href="demo.html" target="_blank"> 我是一个超链接</a>
```

<br/>

<br/>

<br/>

可以在内联框架标签中设置一个`name`属性值,链接将会在指定的内联框架中打开

```html
<!-- 内联框架中打开 -->
<iframe src="demo.html name="tom"></iframe>
<a href="demo.html" target="tom">我在内联框架显示</a>
```

<br/>

<br/>

`center`标签中的内容,会默认在页面中居中显示

如果我们要内容居中显示,要把内容都放到center标签中

```html
<!-- 居中显示 -->
<center>
    a
    aaaaaa
    aaaaaa
</center>
```

<br >

<br>

如果`href`中地址不确定,可以先用`#`代替

如果将链接地址设置为#,点击后会自动回到页面顶部

```html
<!--回到页面顶部-->
<a href="#" target="tom">我在内联框架显示</a>
```

<br >

<br>

html中有一个属性,每一个元素都可以设置,该属性可以作为标签的唯一标识

这个属性就是`id`,id属性在同一个页面中只能有一个,`不能有重复`

<br>

<br>

<br>

跳转到id为bottom的元素所在的位置

直接在`href`中写 `#id`属性值

```html
<a  href="#bottom">回到底部</a>


<!--跳到这里-->
<a id="bottom" href="https://www.baidu.com">哈哈哈</a>
```

