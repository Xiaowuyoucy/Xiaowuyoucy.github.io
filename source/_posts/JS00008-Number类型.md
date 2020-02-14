---
title: JS_Number类型
date: 2020-02-14 08:29:49
tags:
categories: JavaScript
doc:
---

# JS_Number类型

### 基本概念及使用

在`JS`中所有的数值都是`Number`类型,包括整数和浮点数

可以使用一个运算符` typeof`来检查变量类型,使用格式`typeof 变量名`会返回一个变量类型的字符串

`JS`中可以表示的数值的最大值是:`Number.MAX_VALUE`

`JS`中可以表示大于0的最小值`Number.MIN_VALUE`

如果使用`Number`类型的变量超过了`Number.MAX_VALUE`则会返回一个`Infinity`,表示正无穷.

`Infinity`表示正无穷

`-Infinity`表示符无穷

使用`typeof`检查`Infinity`也会返回`Number`

`NaN `是一个特殊的数字,表示Not A Number

用`typeof`检查`NaN`时也会返回`number`

### 精度:

在`JS`中整数的基本运算可以保证精确的.

如果使用`JS`进行浮点运算,可能会得到一个不精确的结果

所以不要使用`JS`进行对精确度要求比较高的运算



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script>
			//声明一个整型变量和浮点型变量
			var intNumber = 100;
			var floatNumber = 1.23;
			
			//在控制台输出两个变量的值
			console.log(intNumber);
			console.log(floatNumber);
			
			//在控制台输出两个变量的类型
			console.log(typeof intNumber);
			console.log(typeof floatNumber);
			
			//在控制台输出最大数值和>0的最大数值
			console.log(Number.MAX_VALUE);
			console.log(Number.MIN_VALUE);
			
			//超过最大值会返回一个Infinity
			var a = Number.MAX_VALUE;
			console.log(a * a);
			console.log(Number.MIN_VALUE * 1 / 2);
			
			//Infinity也是属于Number类型
			console.log(typeof Infinity); 
			
			//会返回一个NaN,表示不是一个数字
			console.log("basd" * "abad"); 
			
			
		</script>
	</head>
	<body>
	</body>
</html>

```

