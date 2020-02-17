---
title: 强制类型转换-Number
date: 2020-02-17 15:48:04
tags:
categories: JS
doc:
---

# 强制类型转换-Number

将其他数据类型转换为``Number`

字符串---->数字

#### 转换方式一:

使用``Number()``函数;

`Number(变量)`

1.如果是纯数字的字符串,则直接将其转换为数字

2.如果是字符串中有非数字的内容,则转换为NaN

3.如果字符串是一个空串或者是一个全空格的字符串,则转换为0

4.布尔类型转Number,true会转换为1,false转换为0

5.null ----> 数字0

6.Undefined ---> 数字:NaN

#### 转换方式二:

这种方式专门用来转换字符串

`parseInt()`把一个字符串转换为一个整数.

`parseInt()`可以将一个字符串中的有效的整数内容取出来.

```javascript
var a = "1234px";
var b = parseInt(a);//1234
```

`parseFloat()`把一个字符串转换为一个浮点数.

```javascript
var a = "1234.111px";
var b = parseInt(a);//1234.111
```

如果对非``String`使用`parseInt()``或``parseFloat()`,他会先将其转换为String,然后在操作

```javascript
var a = true;
var b = perseInt(true);//返回NaN
```

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<script>
            //Number()
			var a = "123";
			a = Number(a);
			console.log(a); //123
			
			a = true;
			a = Number(a);
			console.log(a);//1
			
			a = NaN;
			a = Number(a);
			console.log(a);//NaN
			
			a = undefined;
			a = Number(a);
			console.log(a);//NaN
			
			
			a = null;
			a = Number(a);
			console.log(a);// 0
			
			
			
			
			
			//parseInt() || parseFloat();
			console.log("=========================");
			var a = "123a";
			a = parseInt(a);
			console.log(a);//123
			
			
			var a = "123";
			a = Number(a);
			console.log(a);//123
			
			a = true;
			a = Number(a);
			console.log(a);//1
			
			a = NaN;
			a = Number(a);
			console.log(a);//NaN
			
			a = undefined;
			a = Number(a);
			console.log(a);//NaN
			
			
			a = null;
			a = Number(a);
			console.log(a);//0
			
		</script>
	</body>
</html>

```

