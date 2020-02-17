---
title: 强制类型转换-String
date: 2020-02-17 15:30:25
tags:
categories: JS
doc:
---

#  强制类型转换-String

强制转换类型是指将一个数据类型强制转换为其他的数据类型

类型转换主要指,将其他的数据类型,转换为

String&emsp;&emsp;&emsp;Number&emsp;&emsp;&emsp; Boolean



### 方式一:

调用被转换数据类型的toString()方法

例如 : 

`a.toString()`

该方法不会影响到原变量,他会将转换结果返回.

#### 注意:

如果将null和Undefined这两个值用转换toString()方法就会出错

### 方式二

调用String()函数,并将转换的数据作为参数传递函数

例如:`String(123);`----`String(123);`

使用String();函数做强制转换时:

对于Number和Boolean实际上就是调用toString()方法

但是对于null和undefined,就不会调用toString()方法

他会将null直接转换为"null"

将Undefined直接转换为"Undefined"



```JavaScript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
	<script>
		var str = 123; 
		console.log(str.toString()); 
////	str = null; 出错
//		console.log(str.toString());
//		str = undefined;出错
//		console.log(str.toString());
		str = 123.3;
		console.log(str.toString());
		str = true;
		console.log(str.toString());
		
		
		
		str = 1231;
		str = String(str);
		console.log(str);
		
		console.log("=================");
		str = 123.2;
		str = String(str);
		console.log(str);
		
		
		str = null;
		str = String(str);
		console.log(str);
		
		
		str = undefined;
		str = String(str);
		console.log(str);
		 
		
		str = null;
		str = String(str);
		console.log(str);
		
		
		str = true;
		str = String(str);
		console.log(str);
		
		
	</script>	
		
	</body>
</html>

```

