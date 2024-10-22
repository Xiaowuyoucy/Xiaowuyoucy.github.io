---
title: 内联框架
date: 2020-02-18 06:29:31
tags:
categories: HTML/CSS
doc:
---

# 内联框架

##### 使用内联框架可以引入一个外部的页面.

使用iframe来创建一个内联框架

<br/>

##### 属性:

&emsp;src:指向一个外部页面的路径,可以使用相对路径

&emsp;width:设置宽度

&emsp;height:设置高度

不会按比例缩放

&emsp;name:可以为内联框架指定一个name属性(给框架起名字)

<br/>

<br/>

<br/>

`在现实开发中不推荐使用内联框架`,因为内联框架中的内容不会被搜索引擎所检索,但可以在内网使用;

一般放在body标签内

<br/>

<br/>

格式:`<iframe src="目标路径" name="名字">`

```html
<iframe src="demo.html" name="tom">
```

