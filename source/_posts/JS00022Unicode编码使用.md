---
title: JS中Unicode编码使用
date: 2020-03-04 15:20:38
tags:
categories: JS
doc:
---

# JS中Unicode编码使用

### 在JS中使用Unicode编码.

格式:&emsp;`\u编码 `(十六进制)

编码一般都是十六进制的

### 在html中使用unicode编码.

格式:&emsp;`&#编码;`(十进制)

后面一定要有分号.

```js
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script type="text/javascript">
			
			/*
			 * 在字符串中使用转义字符输入Unicode编码
			 * 	\u四位编码
			 */
			console.log("\u2620");
			
		</script>
	</head>
	<body>
		
		<!--在网页中使用Unicode编码
			&#编码; 这里的编码需要的是10进制
		-->
		<h1 style="font-size: 200px;">&#9760;</h1>
		<h1 style="font-size: 200px;">&#9856;</h1>
		
	</body>
</html>

```

