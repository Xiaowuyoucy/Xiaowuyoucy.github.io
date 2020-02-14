---
title: JavaScript的编写位置
date: 2020-02-14 00:32:04
tags:
categories: JS
doc:
---

# JavaScript的编写位置

##### 可以将js代码写在button标签中的onclick属性中

`<button onclick="alert('我爱你');" >点我一下</button>`

##### 可以将js代码写在超链接的href属性中,这样当点击超链接时,就会执行js代码.

`		<a href="javascript:alert('我太爱你了');">1234</a>`

`		<a href="javascript:;">1234</a>`

##### 可以将js代码写到script标签&nbsp;(&nbsp;推荐使用的方式&nbsp;)

```JavaScript
		<script type="text/javascript">
		/*代码块*/
		</script>
```

##### 可以将js 代码编写到外部js文件中,然后通过script标签引入

##### 外部文件后缀名以.js结尾

##### js代码写到外部文件中可以在不同的页面中同时引用,也可以利用浏览器的缓存机制.

##### 注意:

##### `<Script>`标签一旦用于引入外部文件了,就不能再编写代码在标签内部了,即使编写了浏览器也会忽略

##### 如果需要插入其他js代码,则可以再创建一个新的`script`标签用于编写内部代码

```javascript
		<script type="text/javascript" src="demo001.js">
			
		</script>
```

