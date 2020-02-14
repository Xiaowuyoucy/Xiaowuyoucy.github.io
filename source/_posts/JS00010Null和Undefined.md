---
title: Null和Undefined
date: 2020-02-14 09:22:09
tags: [Null和Undefined]
categories: JS
doc: 
---

# Null和Undefined

`Null(空值)`类型只有一个值,就是`null`

`null`这个值专门用来表示一个为空的对象

使用`typeof`检查一个`null`值时,会返回`object`

`Undefined(未定义)`类型的值只有一个,就`Undefined`

当声明一个变量,但未初始化时,他的值就是`Undefined`

使用`typeof`检查一个`Undefined`值时,也会返回一个`undefined`

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script>
			//定义一个空类型变量,和一个Undefined变量
			var a = null;
			var b;
			
		
			console.log(a); // 输出object
			console.log(b); // 输出undefined
			
			//输出object
			console.log(typeof a);
			//输出 undefined
			console.log(typeof b);
			
		</script>
	</head>
	<body>
	</body>
</html>

```

