---
title: 乱码问题
date: 2020-02-14 21:08:01
tags:
categories: HTML/CSS
doc:
---

# 乱码问题

### 编码:

依据一定的规则,将字符转换为二进制编码的过程称之为编码.

### 解码:

依据一定的规则,将二进制编码转换为字符的过程称之为解码.

### 字符集:

编码和解码所采用的规则,我们称之为字符集

##### &emsp;&emsp;常见字符集:

##### &emsp;&emsp;&emsp;&emsp;1.ASCII

##### &emsp;&emsp;&emsp;&emsp;2.ISO-8859-1

##### &emsp;&emsp;&emsp;&emsp;3.GBK

##### &emsp;&emsp;&emsp;&emsp;4.GB2312&emsp;中文系统默认编码

##### &emsp;&emsp;&emsp;&emsp;5.UTF-8&emsp;万国码,支持地球上所有的文字

### 乱码原因:

产生乱码的根本原因是,编码和解码所采用的字符集不同

在中文系统的浏览器中,默认都是使用GB2312进行解码的

### 解决:

用`<meta charset="字符集" />`标签告诉浏览器网页所采用的编码字符集

meta标签用来设置网页的一些元数据,比如网页的字符集,关键字,简介等..

meta是一个自结束标签,编写一个自结束标签时,可以在开始标签中添加一个 /

### ANSI

ANSI代表智能字符集,会跟随操作系统默认选择的字符集

例如中文操作系统默认的字符集是GB2312

![1581686657431](/images/javawz/1581686657431-1581686663189.png)

```html
<!DOCTYPE html>
<html>
	<head>
        <!--告诉浏览器,使用UTF-8进行解码 -->
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
	</body>
</html>

```



