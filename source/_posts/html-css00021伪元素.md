---
title: 伪元素
date: 2020-06-04 12:12:31
tags:
categories: HTML/CSS
doc:
---

# 伪元素

`:first-letter`

首字符伪元素

修改第一个字符的样式

```css
元素名:first-letter{
}
```

`:first-line`

首行伪元素

修改首行的样式

```css
元素名::first-line{
}
```

`:before`

表示元素最前面部分

`:before`和`:after`通常搭配`content`来使用

```css
p:before{
content: "2133";
}
```

`:after`

 表示元素最后面

```css
p:after{
content: "2133";
}
```



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style>
			p:first-letter{
				color: red;
				font-size: 50px;
			}
			p:first-line{
				background-color: saddlebrown;
			}
			p:before{
				font-size: 50px;
				content: "www";
			}
			p:after{
				font-size: 50px;
				content: ".com";
				color: green;
				

			}

		</style>
	</head>
	<body>
		
		<p>12342112423141242</p>
		
	</body>
</html>

```

