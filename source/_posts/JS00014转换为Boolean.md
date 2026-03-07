---
title: 转换为Boolean
date: 2020-02-17 16:58:33
tags:
categories: JS
doc:
---

# 转换为Boolean

### 将其他类型转换为Boolean

使用`Boolean()函数`

除了0和NaN,其他的都为`true`

字符串转布尔,除了空串,其余的都是`true.`

`null`和`undefined`都会转换为`false`

对象也会转换为`true`

```js
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script type="text/javascript">
			
			/*
			 * 将其他的数据类型转换为Boolean
			 * 	- 使用Boolean()函数
			 * 		- 数字 ---> 布尔
			 * 			- 除了0和NaN，其余的都是true
			 * 
			 * 		- 字符串 ---> 布尔
			 * 			- 除了空串，其余的都是true
			 * 
			 * 		- null和undefined都会转换为false
			 * 
			 * 		- 对象也会转换为true
			 * 		
			 */
			
			var a = 123; //true
			a = -123; //true
			a = 0; //false
			a = Infinity; //true
			a = NaN; //false
			
			//调用Boolean()函数来将a转换为布尔值
			a = Boolean(a);
			
			a = " ";
			
			a = Boolean(a);
			
			a = null; //false
			a = Boolean(a);
			
			a = undefined; //false
			a = Boolean(a);
			
			console.log(typeof a);
			console.log(a);
			
		</script>
	</head>
	<body>
	</body>
</html>

```

<br />

<br />

<br />

<br />

<br />