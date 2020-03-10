---
title: 内联和块元素
date: 2020-03-07 22:11:22
tags:
categories: HTML/CSS
doc:
---

# 内联和块元素

### 块元素和内联元素

##### `div`就是一个块元素

所谓的块元素就是会独占一行的元素,无论他的内容有多少他都会独占一行.

常见的块元素:

`p h1 h2 h3 ...`

`div`这个标签没有任何语义,就是一个纯粹的块元素,
并且不会为它里边的元素设置任何的默认样式
`div`元素主要用来对页面进行布局的

##### `span`是一个内联元素(行内元素)

所谓的行内元素,指的是只占自身大小的元素,不会占用一行.

常见的内联元素:
`a img iframe span`

`span`没有任何的语义,`span`标签专门用来选中文字,
然后为文字来设置样式.

块元素主要用来做页面布局的,内联元素主要用来选中文本设置样式.
一般情况下只使用块元素去包含内联元素,而不会使用内联元素去包含一个块元素
`a`元素可以包含任意元素,除了他本身

`p`元素不可以包含任何块元素

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8" />
	<title>Document</title>
</head>
<body>
	<div style="background-color:red;">我是一个块元素</div>
	<span style="background-color:blue;">我是一个内联元素</span>
	<a href="#">
		<div>我被a元素包含</div>
	</a>
	
	
	<!-- 错误写法 -->
	<p>
		<p>我是错误写法</p>
	</p>
	
	<span>
		<div>
			我是错误写法
		</div>
	</span>
	
</body>
</html>
```

