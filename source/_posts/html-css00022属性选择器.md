---
title: 属性选择器
date: 2020-06-04 12:39:16
tags:
categories: HTML/CSS
doc:
---

# 属性选择器

在`HBuilder`中按`ctrl+shift+f`可以折叠代码

`title`属性

可以给标签起一个标题名字

鼠标放上去会显示属性值

```html
<p title="123">zzz</p>
```

元素名[属性名] 选择含有指定属性的元素

```css
p[title]{
color: yellow;
}
p[class]{
color: yellow;
}
```

元素名[属性名=属性值] 选择含有指定属性和属性值的元素

```css
p[title="123"]{
color: yellow;
}
p[class="111"]{
color: yellow;
}
```



元素名[属性名^=属性值] 选择属性值以指定值开头的元素



```css
p[title^="a"]{
color: yellow;
}
p[class^="b"]{
color: yellow;
}
```



元素名[属性名$=属性值] 选择属性值以指定值结尾的元素

```css
p[title$="a"]{
color: yellow;
}
p[class$="b"]{
color: yellow;
}
```



元素名[属性名*=属性值] 选择属性值中含有某值的元素的元素

```css
p[title*="a"]{
color: yellow;
}
p[class*="b"]{
color: yellow;
}
```

```html
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			p[class]{
				background-color: yellow;
			}
			p[title="a"]{
				background-color: red;
			}
			p[title^="ac"]{
				background-color: green;
			}
			p[title$="bc"]{
				color: red;
			}
			p[title*="z"]{
				color: brown;
			}
		</style>
	</head>

	<body>
		<p title="acc">123</p>
		<p title="acc">123</p>
		<p title="abc">123</p>
		<p title="abc">123</p>
		<p title="a">123</p>
		<p class="bc">123</p>
		<p class="bc">123</p>
		<p title="azs">123</p>
		<p title="aza">123</p>
		<p title="azc">123</p>
	</body>

</html>
```

