---
title: 后代选择器和子元素选择器
date: 2020-05-30 17:52:56
tags:
categories: HTML/CSS
doc:
---

# 后代选择器和子元素选择器

##### 父元素

 直接包含子元素的元素叫做父元素

##### 子元素

直接被父元素包含的元素是子元素

##### 祖先元素

​    直接或间接包含后代元素的元素叫做祖先元素
​            一个元素的父元素也是它的祖先元素

##### 后代元素

​    直接或间接被祖先元素包含的元素叫做后代元素
​            子元素也是后代元素

#####  兄弟元素

拥有相同父元素的元素是兄弟元素





### 后代元素选择器

   作用：选中指定元素内的指定后代元素
           语法：

```css
祖先 后代{
}
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style>
			div span{
				background-color: red;
			}
		</style>
	</head>
	<body>
		<div>
			<span>我是div的后代</span>
			<p>我也是div的后代</p>
			<p><span>我span也是div的后代</span></p>
		</div>
	</body>
</html>

```

### 子元素选择器

​        作用：选中指定父元素的指定子元素
​                语法：

```css
父元素>子元素{

}
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style>
			div>span{
				background-color:red;
			}
		</style>
	</head>
	<body>
		<div>
			<span>我是div的直接子元素</span>
			<p><span>我span不是div的直接子元素</span></p>
		</div>
	</body>
</html>

```

