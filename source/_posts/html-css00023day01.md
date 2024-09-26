---
title: day01
date: 2024-09-12 08:18:25
tags:
categories: HTML/CSS
doc:
---

### 子元素选择器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/*
			 * 为第一个p标签设置一个背景颜色为黄色
			 * 	:first-child 可以选中第一个子元素
			 *  :last-child 可以选中最后一个子元素
			 */
			/*body > p:first-child{
				background-color: yellow;
			}*/
			
			/*p:last-child{
				background-color: yellow;
			}*/
			
			/*
			 * :nth-child 可以选中任意位置的子元素
			 * 		该选择器后边可以指定一个参数，指定要选中第几个子元素
			 * 		even 表示偶数位置的子元素
			 * 		odd 表示奇数位置的子元素
			 * 		
			 */
			/*p:nth-child(odd){
				background-color: yellow;
			}*/
			
			/*
			 * :first-of-type
			 * :last-of-type
			 * :nth-of-type
			 * 		和:first-child这些非常的类似，
			 * 		只不过child，是在所有的子元素中排列
			 * 		而type，是在当前类型的子元素中排列
			 */
			/*p:first-of-type{
				background-color: yellow;
			}*/
			p:last-of-type{
				background-color: yellow;
			}
		</style>
	</head>
	<body>
		<span>我是span</span>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<span>hello</span>
		
		<!--<div>
			<p>我是DIV中的p标签</p>
		</div>-->
		
	</body>
</html>

```

<br/>

<br/>

### 兄弟元素选择器

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			/*
			 * 为span后的一个p元素设置一个背景颜色为黄色
			 * 后一个兄弟元素选择器
			 * 	作用：可以选中一个元素后紧挨着的指定的兄弟元素
			 * 	语法：前一个 + 后一个
			 * 
			 */
			
			/*span + p{
				background-color: yellow;
			}*/
			
			/*
			 * 选中后边的所有兄弟元素
			 * 	语法：前一个 ~ 后边所有	
			 */
			span ~ p{
				background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<span>我是一个span</span>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
	</body>
</html>

```

<br/>

<br/>

### 否定伪类

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			/*
			 * 为所有的p元素设置一个背景颜色为黄色，除了class值为hello的
			 * 
			 * 否定伪类：
			 * 	作用：可以从已选中的元素中剔除出某些元素
			 * 	语法：
			 * 		:not(选择器)
			 */
			p:not(.hello){
				background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
		<p class="hello">我是一个p标签</p>
		<p>我是一个p标签</p>
		<p>我是一个p标签</p>
	</body>
</html>
	
```

<br/>

<br/>

### 样式的继承

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			body{
				font-size: 30px;
			}
		</style>
	</head>
	<body>
		<!-- 
			像儿子可以继承父亲的遗产一样，在CSS中，祖先元素上的样式，也会被他的后代元素所继承,
			利用继承，可以将一些基本的样式设置给祖先元素，这样所有的后代元素将会自动继承这些样式。
			
			但是并不是所有的样式都会被子元素所继承，比如：背景相关的样式都不会被继承 边框相关的样式 定位相关的
		-->
		
		<div style="background-color: yellow;">
			<p>
				我是p标签中的文字
				<span>我是span中的文字</span>
			</p>
		</div>
		
		
		<span>我是p元素外的span</span>
		
	</body>
</html>

```



<br/>

<br/>

### 选择器的优先级

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.p1{
				background-color: yellow;
			}
			
			p{
				background-color: red;
			}
			
			
			
			/*
			 * 当使用不同的选择器，选中同一个元素时并且设置相同的样式时，
			 * 	这时样式之间产生了冲突，最终到底采用哪个选择器定义的样式，由选择器的优先级（权重）决定
			 *  优先级高的优先显示。
			 * 
			 * 优先级的规则
			 * 		内联样式 ， 优先级  1000
			 * 		id选择器，优先级   100
			 * 		类和伪类， 优先级   10
			 * 		元素选择器，优先级 1 
			 * 		通配* ，    优先级 0
			 * 		继承的样式，没有优先级
			 * 
			 * 当选择器中包含多种选择器时，需要将多种选择器的优先级相加然后在比较，
			 * 	但是注意，选择器优先级计算不会超过他的最大的数量级，如果选择器的优先级一样，
			 * 	则使用靠后的样式。
			 * 
			 *  并集选择器的优先级是单独计算
			 * 	div , p , #p1 , .hello{}	
			 * 
			 *  可以在样式的最后，添加一个!important，则此时该样式将会获得一个最高的优先级，
			 * 	将会优先于所有的样式显示甚至超过内联样式，但是在开发中尽量避免使用!important
			 * 
			 */
			
			*{
				font-size: 50px;
			}
			
			p{
				font-size: 30px;
			}
			
			#p2{
				background-color: yellowgreen;
			}
			
			p#p2{
				background-color: red;
			}
			
			
			.p3{
				color: green;
			}
			
			.p1{
				color: yellow;
				background-color: greenyellow !important;
			}
			
			
			
		</style>
	</head>
	<body>
		
		<p class="p1 p3" id="p2" style="background-color: orange;">我是一个段落
			<span>我是p标签中的span</span>
		</p>
		
	</body>
</html>

```



<br/>

<br/>

### a的伪类

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
		
			/*
			 * 涉及到a的伪类一共有四个：
			 * 	:link     未访问
			 *  :visited  已访问
			 * 	:hover    鼠标移入
			 * 	:active   正在点击
			 * 而这四个选择器的优先级是一样的。
			 */
			
			a:link{
				color: yellowgreen;
			}
			
			a:visited{
				color: red;
			}
			
			/*
			 * 鼠标移入
			 */
			a:hover{
				color: orange;
			}
			
			/*
			 * 正在点击
			 */
			a:active{
				color: cornflowerblue;
			}
			
			
			
			
			
		</style>
	</head>
	<body>
		
		<a href="http://www.baidu.com">访问过的链接</a>
		<br /><br />
		<a href="http://www.baidu123456.com">未访问过的链接</a>
		
	</body>
</html>

```

<br/>

<br/>

### 文本标签

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<!-- 
			em和strong
			- 这两个标签都表示一个强调的内容，
				em主要表示语气上的强调,em在浏览器中默认使用斜体显示
				strong表示强调的内容，比em更强烈，默认使用粗体显示
		-->
		<p>
			今天天气<em>真不错</em>！
		</p>
		
		<p>
			<strong>
				注意：如果你不认真上课，你就找不到好工作！
			</strong>
		</p>
		
		<!-- 
			i标签中的内容会以斜体显示
			b标签中的内容会以加粗显示
			
			h5规范中规定，对于不需要着重的内容而是单纯的加粗或者是斜体，
				就可以使用b和i标签
		-->
		<p>
			<i>我是i标签中的内容</i>
			<b>我是b标签中的内容</b>
		</p>
		
		<!--
			small标签中的内容会比他的父元素中的文字要小一些
				在h5中使用small标签来表示一些细则一类的内容
				比如：合同中小字，网站的版权声明都可以放到small
		-->
		<p>
			我是p标签中的内容<small>我是small标签中的内容</small>
		</p>
	
		<!-- 
			网页中所有的加书名号的内容都可以使用cite标签，表示参考的内容，
				比如：书名 歌名 话剧名 电影名 。。。
		-->
		<p>
			<cite>《论语》</cite>是最喜欢的一般的书
		</p>
		
		<!--
			q标签表示一个短的引用（行内引用）
				q标签引用的内容，浏览器会默认加上引号
			
			blockquote标签表示一个长引用（块级引用）
		-->
		<p>
			子曰:<q>学而时习之不亦说乎！</q>
		</p>
		
		<div>
			子曰:
			<blockquote>
				有朋自远方来，乐呵乐呵！
			</blockquote>
		</div>
		
		<!-- 
			使用sup标签来设置一个上标
		-->
		<p>2<sup>2</sup></p>
		<p>赵薇<sup><a href="#">[1]</a></sup></p>
		
		<!--
			sub标签用来表示一个下标
		-->
		<p>H<sub>2</sub>O</p>
		
		<!--
			使用del标签来表示一个删除的内容
				del标签中的内容，会自动添加删除线
		-->
		<p>
			<del>17.75</del> <br />
			15.54 <br />
		</p>
		
		
		<!-- 
			ins表示一个插入的内容
				ins中的的内容，会自动添加下划线
		-->
		<p>
			我们的老师真<ins>好啊</ins>！
		</p>
		
		
		<!--
			需要页面中直接编写一些代码
			pre是一个预格式标签，会将代码中的格式保存，不会忽略多个空格
			code专门用来表示代码
			
			我们一般结合使用pre和code来表示一段代码
		-->
		
		<pre>
			<code>
				window.onload = function(){
					alert("Hello World");
				};
			</code>
		</pre>
		
		
	</body>
</html>

```

<br/>

<br/>

### 列表

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			/*
			 	去掉项目符号
			 * */
			/*ul{
				list-style: none;
			}*/
			
		</style>
	</head>
	<body>
		
		<!-- 
			列表就相当于去超市购物时的那个购物清单，
				在HTML也可以创建列表，在网页中一共有三种列表：
					1.无序列表
					2.有序列表
					3.定义列表
		-->
		
		<!--
			无序列表
				- 使用ul标签来创建一个无序列表
				- 使用li在ul中创建一个一个的列表项，
					一个li就是一个列表项
					
			通过type属性可以修改无序列表的项目符号
				可选值：
					disc，默认值，实心的圆点
					square，实心的方块
					circle，空心的圆
					
			注意：默认的项目符号我们一般都不使用！！
				如果需要设置项目符号，则可以采用为li设置背景图片的方式来设置
				
			ul和li都是块元素	
		-->
		<ul>
			<li>西门大官人</li>
			<li>柴大官人</li>
			<li>许大官人</li>
			<li>唐僧大官人</li>
		</ul>
		
		<!-- 
			有序列表和无序列表类似，只不过它使用ol来代替ul
			有序列表使用有序的序号作为项目符号
			type属性，可以指定序号的类型
				可选值：1，默认值，使用阿拉伯数字
						a/A 采用小写或大写字母作为序号
						i/I 采用小写或大写的罗马数字作为序号
						
			ol也是块元素			
		-->
		<ol type="I">
			<li>结构</li>
			<li>表现</li>
			<li>行为</li>
		</ol>
		
		<!-- 
			列表之间都是可以互相嵌套，可以在无序列表中放个有序列表
				也可以在有序列表中放一个无序列表
		-->
		
		<p>菜谱</p>
		<ul>
			<li>
				鱼香肉丝
				<ol>
					<li>鱼</li>
					<li>香</li>
					<li>肉丝</li>
				</ol>
			</li>
			<li>
				宫保鸡丁
				<ul>
					<li>宫保</li>
					<li>鸡丁</li>
				</ul>
			</li>
			<li>青椒肉丝</li>
		</ul>
		
	</body>
</html>

```

<br/>

<br/>

### 定义列表

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		
	</head>
	<body>
		<!--
			定义列表用来对一些词汇或内容进行定义
			使用dl来创建一个定义列表
				dl中有两个子标签
					dt ： 被定义的内容
					dd ： 对定义内容的描述
			同样dl和ul和ol之间都可以互相嵌套		
		-->
		<dl>
			<dt>武松</dt>
			<dd>景阳冈打虎英雄，战斗力99</dd>
			<dd>后打死西门庆，投奔梁山</dd>
			<dt>武大</dt>
			<dd>著名餐饮企业家，战斗力0</dd>
		</dl>
	</body>
</html>

```

<br/>

<br/>

### 单位

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			/*
			 * 长度单位
			 * 		像素 px
			 * 			- 像素是我们在网页中使用的最多的一个单位，
			 * 				一个像素就相当于我们屏幕中的一个小点，
			 * 				我们的屏幕实际上就是由这些像素点构成的
			 * 				但是这些像素点，是不能直接看见。
			 * 			- 不同显示器一个像素的大小也不相同，
			 * 				显示效果越好越清晰，像素就越小，反之像素越大。
			 * 
			 * 		百分比 %
			 * 			- 也可以将单位设置为一个百分比的形式，
			 * 				这样浏览器将会根据其父元素的样式来计算该值
			 * 			- 使用百分比的好处是，当父元素的属性值发生变化时，
			 * 				子元素也会按照比例发生改变
			 * 			- 在我们创建一个自适应的页面时，经常使用百分比作为单位
			 * 
			 * 		em
			 * 			- em和百分比类似，它是相对于当前元素的字体大小来计算的
			 * 			- 1em = 1font-size
			 * 			- 使用em时，当字体大小发生改变时，em也会随之改变
			 * 			- 当设置字体相关的样式时，经常会使用em
			 * 			
			 */
			.box{
				width: 300px;
				height: 300px;
				background-color: red;
			}
			
			.box1{
				font-size: 20px;
				width: 2em;
				height: 50%;
				background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		
		<div class="box">
			
			<div class="box1"></div>
			
		</div>
		
	</body>
</html>

```

<br/>

<br/>

### 颜色单位

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 100px;
				height: 100px;
				
				/*
				 * 颜色单位：
				 * 	 在CSS可以直接使用颜色的单词来表示不同的颜色
				 * 		红色：red
				 * 		蓝色：blue
				 * 		绿色：green	
				 *   也可以使用RGB值来表示不同的颜色
				 * 		- 所谓的RGB值指的是通过Red Green Blue三元色，
				 * 			通过这三种颜色的不同的浓度，来表示出不同的颜色
				 * 		- 例子：rgb(红色的浓度,绿色的浓度,蓝色的浓度);
				 * 			- 颜色的浓度需要一个0-255之间的值，255表示最大，0表示没有
				 * 			- 浓度也可以采用一个百分数来设置，需要一个0% - 100%之间的数字
				 * 				使用百分数最终也会转换为0-255之间的数
				 * 				0%表示0
				 * 				100%表示255
				 *   也可以使用十六进制的rgb值来表示颜色，原理和上边RGB原理一样，
				 * 		只不过使用十六进制数来代替，使用三组两位的十六进制数组来表示一个颜色
				 * 		每组表示一个颜色	,第一组表示红色的浓度，范围00-ff
				 * 					第二组表示绿色的浓度，范围是00-ff
				 * 					第三组表示蓝色的浓度，范围00-ff
				 * 		语法：#红色绿色蓝色
				 * 		十六进制：
				 * 		0 1 2 3 4 5 6 7 8 9 a b c d e f
				 * 		00 - ff
				 * 		00表示没有，相当于rgb中的0
				 * 		ff表示最大，相当于rgb中255
				 * 		红色：
				 * 			#ff0000
				 * 		像这种两位两位重复的颜色，可以简写
				 * 			比如：#ff0000 可以写成 #f00
				 * 			#abc  #aabbcc		
				 * 			
				 */
				/*background-color: rgb(161,187,215);*/
				
				/*background-color: rgb(100%,50%,50%);*/
				
				/*background-color: #00f;*/
				
				/*background-color: #abc;*/ /*#aabbcc*/
				
				background-color: #084098;
			}
			
		</style>
	</head>
	<body>
		<div class="box1"></div>
	</body>
</html>

```

<br/>

<br/>

### 字体样式

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.p1{
				/*设置字体颜色,使用color来设置文字的颜色*/
				color: red;
				/*
				 * 设置文字的大小,浏览器中一般默认的文字大小都是16px
				 	font-size设置的并不是文字本身的大小，
				 		在页面中，每个文字都是处在一个看不见的框中的
				 		我们设置的font-size实际上是设置格的高度，并不是字体的大小
				 		一般情况下文字都要比这个格要小一些，也有时会比格大，
				 		根据字体的不同，显示效果也不能	
				 * */
				font-size: 30px;
				
				/*
				 * 通过font-family可以指定文字的字体
				 * 	当采用某种字体时，如果浏览器支持则使用该字体，
				 * 		如果字体不支持，则使用默认字体
				 * 该样式可以同时指定多个字体，多个字体之间使用,分开
				 * 	当采用多个字体时，浏览器会优先使用前边的字体，
				 * 		如果前边没有在尝试下一个
				 */
				/*font-family: arial , 微软雅黑;*/
				
				/*
				 * 浏览器使用的字体默认就是计算机中的字体，
				 * 	如果计算机中有，则使用，如果没有就不用
				 * 
				 * 在开发中，如果字体太奇怪，用的太少了，尽量不要使用，
				 * 	有可能用户的电脑没有，就不能达到想要的效果。
				 */
				/*font-family: 华文彩云 , arial , 微软雅黑;*/
				
				font-family: "curlz mt";
				
			}
			
		</style>
	</head>
	<body>
		
		<p class="p1">
			我是一个p标签，ABCDEFGabcdefg
		</p>
		
	</body>
</html>

```

<br/>

<br/>

### 字体的分类

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			p{
				font-family: arial , 微软雅黑 , 华文彩云 , serif;
			}
		</style>
	</head>
	<body>
		<!-- 
			在网页中将字体分成5大类：
				serif（衬线字体）
				sans-serif（非衬线字体）
				monospace （等宽字体）
				cursive （草书字体）
				fantasy （虚幻字体）
			可以将字体设置为这些大的分类,当设置为大的分类以后，
				浏览器会自动选择指定的字体并应用样式
			一般会将字体的大分类，指定为font-family中的最后一个字体	
		-->
		<p style="font-size: 50px; font-family: serif;">衬线字体：我是一段文字，ABCDEFGabcdefg</p>
		<p style="font-size: 50px; font-family: sans-serif;">非衬线字体：我是一段文字，ABCDEFGabcdefg</p>
		<p style="font-size: 50px; font-family: monospace;">等宽字体：我是一段文字，IHABCDEFGabcdefg</p>
		<p style="font-size: 50px; font-family: cursive;">草书字体：我是一段文字，ABCDEFGabcdefg</p>
		<p style="font-size: 50px; font-family: fantasy;">虚幻字体：我是一段文字，ABCDEFGabcdefg</p>
	</body>
</html>

```

<br/>

<br/>

### 字体的其他样式

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.p1{
				color: red;
				font-size: 30px;
				font-family: "微软雅黑";
				/*
				 * font-style可以用来设置文字的斜体
				 * 	- 可选值：
				 * 		normal，默认值，文字正常显示
				 * 		italic 文字会以斜体显示
				 * 		oblique 文字会以倾斜的效果显示
				 * 	- 大部分浏览器都不会对倾斜和斜体做区分，
				 * 		也就是说我们设置italic和oblique它们的效果往往是一样的
				 *  - 一般我们只会使用italic
				 */
				font-style: italic;
				
				/*
				 * font-weight可以用来设置文本的加粗效果：
				 * 		可选值：
				 * 			normal，默认值，文字正常显示
				 * 			bold，文字加粗显示
				 * 
				 * 	该样式也可以指定100-900之间的9个值，
				 * 		但是由于用户的计算机往往没有这么多级别的字体，所以达到我们想要的效果
				 * 		也就是200有可能比100粗，300有可能比200粗，但是也可能是一样的
				 */
				font-weight: bold;
				
				/*
				 * font-variant可以用来设置小型大写字母
				 * 		可选值：
				 * 			normal，默认值，文字正常显示
				 * 			small-caps 文本以小型大写字母显示
				 * 
				 * 小型大写字母：
				 * 		将所有的字母都以大写形式显示，但是小写字母的大写，
				 * 			要比大写字母的大小小一些。
				 */
				font-variant: small-caps ;
			}
			
			.p2{
				/*设置一个文字大小*/
				font-size: 50px;
				/*设置一个字体*/
				font-family: 华文彩云;
				/*设置文字斜体*/
				font-style: italic;
				/*设置文字的加粗*/
				font-weight: bold;
				/*设置一个小型大写字母*/
				font-variant: small-caps;
			}
			
			.p3{
				/*
				 * 在CSS中还为我们提供了一个样式叫font，
				 * 	使用该样式可以同时设置字体相关的所有样式,
				 * 	可以将字体的样式的值，统一写在font样式中，不同的值之间使用空格隔开
				 * 
				 * 使用font设置字体样式时，斜体 加粗 小大字母，没有顺序要求，甚至可写可不写，
				 * 	如果不写则使用默认值，但是要求文字的大小和字体必须写，而且字体必须是最后一个样式
				 * 	大小必须是倒数第二个样式
				 * 
				 * 实际上使用简写属性也会有一个比较好的性能
				 */
				font: small-caps bold italic 60px "微软雅黑";
			}
			
			
		</style>
	</head>
	<body>
		
		<p class="p3">我是一段文字，ABCDEFGabcdefg</p>
		
		<p class="p1">我是一段文字，ABCDEFGabcdefg</p>
		
		<p class="p2">我是一段文字，ABCDEFGabcdefg</p>
	</body>
</html>

```

<br/>

<br/>

### 行间距

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/*
			 * 在CSS并没有为我们提供一个直接设置行间距的方式，
			 * 	我们只能通过设置行高来间接的设置行间距，行高越大行间距越大
			 * 使用line-height来设置行高 
			 * 	行高类似于我们上学单线本，单线本是一行一行，线与线之间的距离就是行高，
			 * 	网页中的文字实际上也是写在一个看不见的线中的，而文字会默认在行高中垂直居中显示
			 * 
			 * 行间距 = 行高 - 字体大小
			 */
			.p1{
				font-size: 20px;
				
				/*
				 * 通过设置line-height可以间接的设置行高，
				 * 	可以接收的值：
				 * 		1.直接就收一个大小
				 * 		2.可以指定一个百分数，则会相对于字体去计算行高
				 * 		3.可以直接传一个数值，则行高会设置字体大小相应的倍数
				 */
				/*line-height: 200%;*/
				
				line-height: 2;
			}
			
			.box{
				width: 200px;
				height: 200px;
				background-color: #bfa;
				/*
				 * 对于单行文本来说，可以将行高设置为和父元素的高度一致，
				 * 	这样可以是单行文本在父元素中垂直居中
				 */
				line-height: 200px;
			}
			
			.p2{
				
				
				/*
				 * 在font中也可以指定行高
				 * 	在字体大小后可以添加/行高，来指定行高，该值是可选的，如果不指定则会使用默认值
				 */
				font: 30px "微软雅黑";
				line-height: 50px;
			}
			
		</style>
	</head>

	<body>
		
		<p class="p2">
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		
		<div class="box">
			
			<a href="#">我是一个超链接</a>
			
		</div>
		

		<p class="p1">
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>

	</body>

</html>
```

<br/>

<br/>

### 文本样式

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			.p1 {
				/*
				 * text-transform可以用来设置文本的大小写
				 * 	可选值：
				 * 		none 默认值，该怎么显示就怎么显示，不做任何处理
				 * 		capitalize 单词的首字母大写，通过空格来识别单词
				 * 		uppercase 所有的字母都大写
				 * 		lowercase 所有的字母都小写
				 */
				text-transform: lowercase;
			}
			
			.p2 {
				/*
				 * text-decoration可以用来设置文本的修饰
				 * 		可选值：
				 * 			none：默认值，不添加任何修饰，正常显示
				 * 			underline 为文本添加下划线
				 * 			overline 为文本添加上划线
				 * 			line-through 为文本添加删除线
				 */
				text-decoration: line-through;
			}
			
			a {
				/*超链接会默认添加下划线，也就是超链接的text-decoration的默认值是underline
				 	如果需要去除超链接的下划线则需要将该样式设置为none
				 * */
				text-decoration: none;
			}
			
			.p3 {
				/**
				 * letter-spacing可以指定字符间距
				 */
				/*letter-spacing: 10px;*/
				
				/*
				 * word-spacing可以设置单词之间的距离
				 * 	实际上就是设置词与词之间空格的大小
				 */
				word-spacing: 120px;
			}
			
			
			.p4{
				/*
				 * text-align用于设置文本的对齐方式
				 * 	可选值：
				 * 		left 默认值，文本靠左对齐
				 * 		right ， 文本靠右对齐
				 * 		center ， 文本居中对齐
				 * 		justify ， 两端对齐
				 * 				- 通过调整文本之间的空格的大小，来达到一个两端对齐的目的
				 */
				text-align: justify ;
			}
			
			.p5{
				
				font-size: 20px;
				
				/*
				 * text-indent用来设置首行缩进
				 * 	当给它指定一个正值时，会自动向右侧缩进指定的像素
				 * 	如果为它指定一个负值，则会向左移动指定的像素,
				 * 		通过这种方式可以将一些不想显示的文字隐藏起来
				 *  这个值一般都会使用em作为单位
				 */
				text-indent: -99999px;
			}
			
		</style>
	</head>

	<body>
		
		<p class="p5">
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		
		<h1 class="p4">我是一个h1</h1>
		
		<p class="p4">
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>

		<p class="p4">
			“We should start back,” Gared urged as the woods began to grow dark around them. “The wildlings are dead.” “Do the dead frighten you?” Ser Waymar Royce asked with just the hint of a smile. Gared did not rise to the bait. He was an old man, past fifty, and he had seen the lordlings come and go. “Dead is dead,” he said. “We have no business with the dead.” “Are they dead?” Royce asked softly. “What proof have we?” “Will saw them,” Gared said. “If he says they are dead, that’s proof enough for me.” Will had known they would drag him into the quarrel sooner or later. He wished it had been later rather than sooner. “My mother told me that dead men sing no songs,” he put in. “My wet nurse said the same thing, Will,” Royce replied. “Never believe anything you hear at a woman’s tit. There are things to be learned even from the dead.” His voice echoed, too loud in the twilit forest. “We have a long ride before us,” Gared pointed out. “Eight days, maybe nine. And night is falling.” Ser Waymar Royce glanced at the sky with disinterest. “It does that every day about this time. Are you unmanned by the dark, Gared?” Will could see the tightness around Gared’s mouth, the barely suppressed anger in his eyes under the thick black hood of his cloak. Gared had spent forty years in the Night’s Watch, man and boy, and he was not accustomed to being made light of. Yet it was more than that. Under the wounded pride, Will could sense something else in the older man. You could taste it; a nervous tension that came perilous close to fear. Will shared his unease. He had been four years on the Wall. The first time he had been sent beyond, all the old stories had come rushing back, and his bowels had turned to water. He had laughed about it afterward. He was a veteran of a hundred rangings by now, and the endless dark wilderness that the southron called the haunted forest had no more terrors for him.
		</p>

		<p class="p3">
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>

		<p class="p3">
			“We should start back,” Gared urged as the woods began to grow dark around them. “The wildlings are dead.” “Do the dead frighten you?” Ser Waymar Royce asked with just the hint of a smile. Gared did not rise to the bait. He was an old man, past fifty, and he had seen the lordlings come and go. “Dead is dead,” he said. “We have no business with the dead.” “Are they dead?” Royce asked softly. “What proof have we?” “Will saw them,” Gared said. “If he says they are dead, that’s proof enough for me.” Will had known they would drag him into the quarrel sooner or later. He wished it had been later rather than sooner. “My mother told me that dead men sing no songs,” he put in. “My wet nurse said the same thing, Will,” Royce replied. “Never believe anything you hear at a woman’s tit. There are things to be learned even from the dead.” His voice echoed, too loud in the twilit forest. “We have a long ride before us,” Gared pointed out. “Eight days, maybe nine. And night is falling.” Ser Waymar Royce glanced at the sky with disinterest. “It does that every day about this time. Are you unmanned by the dark, Gared?” Will could see the tightness around Gared’s mouth, the barely suppressed anger in his eyes under the thick black hood of his cloak. Gared had spent forty years in the Night’s Watch, man and boy, and he was not accustomed to being made light of. Yet it was more than that. Under the wounded pride, Will could sense something else in the older man. You could taste it; a nervous tension that came perilous close to fear. Will shared his unease. He had been four years on the Wall. The first time he had been sent beyond, all the old stories had come rushing back, and his bowels had turned to water. He had laughed about it afterward. He was a veteran of a hundred rangings by now, and the endless dark wilderness that the southron called the haunted forest had no more terrors for him.
		</p>

		<a href="#">我是超链接</a>

		<p class="p2">
			“We should start back,” Gared urged as the woods began to grow dark around them. “The wildlings are dead.” “Do the dead frighten you?” Ser Waymar Royce asked with just the hint of a smile. Gared did not rise to the bait. He was an old man, past fifty, and he had seen the lordlings come and go. “Dead is dead,” he said. “We have no business with the dead.” “Are they dead?” Royce asked softly. “What proof have we?” “Will saw them,” Gared said. “If he says they are dead, that’s proof enough for me.” Will had known they would drag him into the quarrel sooner or later. He wished it had been later rather than sooner. “My mother told me that dead men sing no songs,” he put in. “My wet nurse said the same thing, Will,” Royce replied. “Never believe anything you hear at a woman’s tit. There are things to be learned even from the dead.” His voice echoed, too loud in the twilit forest. “We have a long ride before us,” Gared pointed out. “Eight days, maybe nine. And night is falling.” Ser Waymar Royce glanced at the sky with disinterest. “It does that every day about this time. Are you unmanned by the dark, Gared?” Will could see the tightness around Gared’s mouth, the barely suppressed anger in his eyes under the thick black hood of his cloak. Gared had spent forty years in the Night’s Watch, man and boy, and he was not accustomed to being made light of. Yet it was more than that. Under the wounded pride, Will could sense something else in the older man. You could taste it; a nervous tension that came perilous close to fear. Will shared his unease. He had been four years on the Wall. The first time he had been sent beyond, all the old stories had come rushing back, and his bowels had turned to water. He had laughed about it afterward. He was a veteran of a hundred rangings by now, and the endless dark wilderness that the southron called the haunted forest had no more terrors for him.
		</p>

		<p class="p1">
			“We should start back,” Gared urged as the woods began to grow dark around them. “The wildlings are dead.” “Do the dead frighten you?” Ser Waymar Royce asked with just the hint of a smile. Gared did not rise to the bait. He was an old man, past fifty, and he had seen the lordlings come and go. “Dead is dead,” he said. “We have no business with the dead.” “Are they dead?” Royce asked softly. “What proof have we?” “Will saw them,” Gared said. “If he says they are dead, that’s proof enough for me.” Will had known they would drag him into the quarrel sooner or later. He wished it had been later rather than sooner. “My mother told me that dead men sing no songs,” he put in. “My wet nurse said the same thing, Will,” Royce replied. “Never believe anything you hear at a woman’s tit. There are things to be learned even from the dead.” His voice echoed, too loud in the twilit forest. “We have a long ride before us,” Gared pointed out. “Eight days, maybe nine. And night is falling.” Ser Waymar Royce glanced at the sky with disinterest. “It does that every day about this time. Are you unmanned by the dark, Gared?” Will could see the tightness around Gared’s mouth, the barely suppressed anger in his eyes under the thick black hood of his cloak. Gared had spent forty years in the Night’s Watch, man and boy, and he was not accustomed to being made light of. Yet it was more than that. Under the wounded pride, Will could sense something else in the older man. You could taste it; a nervous tension that came perilous close to fear. Will shared his unease. He had been four years on the Wall. The first time he had been sent beyond, all the old stories had come rushing back, and his bowels had turned to water. He had laughed about it afterward. He was a veteran of a hundred rangings by now, and the endless dark wilderness that the southron called the haunted forest had no more terrors for him.
		</p>
	</body>

</html>
```



<br/>

<br/>

### 盒子模型

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				/*
				 * 使用width来设置盒子内容区的宽度
				 * 使用height来设置盒子内容区的高度
				 * 
				 * width和height只是设置的盒子内容区的大小，而不是盒子的整个大小，
				 * 	盒子可见框的大小由内容区，内边距和边框共同决定
				 */
				width: 300px;
				height: 300px;
				
				/*设置背景颜色*/
				background-color: #bfa;
				
				/*
				 * 为元素设置边框
				 * 	要为一个元素设置边框必须指定三个样式
				 * 		border-width:边框的宽度
				 * 		border-color:边框颜色
				 * 		border-style:边框的样式
				 */
				
				/*
				 * 设置边框的宽度
				 */
				/*border-width:10px ;*/
				
				/*
				 	使用border-width可以分别指定四个边框的宽度
				 	如果在border-width指定了四个值，
				 		则四个值会分别设置给 上 右 下 左，按照顺时针的方向设置的
				 		
				 	如果指定三个值，
				 		则三个值会分别设置给	上  左右 下
				 		
				 	如果指定两个值，
				 		则两个值会分别设置给 上下 左右	
				 		
				 	如果指定一个值，则四边全都是该值	
				 	
				 	除了border-width，CSS中还提供了四个border-xxx-width
				 		xxx的值可能是top right bottom left
				 	专门用来设置指定边的宽度	
				 * */
				/*border-width:10px 20px 30px 40px ;*/
				/*border-width:10px 20px 30px ;*/
				/*border-width: 10px 20px ;*/
				border-width: 10px;
				
				/*border-left-width:100px ;*/
				
				
				/*
				 * 设置边框的颜色
				 * 和宽度一样，color也提供四个方向的样式，可以分别指定颜色
				 * border-xxx-color
				 */
				border-color: red;
				/*border-color: red yellow orange blue;*/
				/*border-color: red yellow orange;*/
				/*border-color: red yellow;*/
				
				/*
				 * 设置边框的样式
				 * 	可选值：
				 * 		none，默认值，没有边框
				 * 		solid 实线
				 * 		dotted 点状边框
				 * 		dashed 虚线
				 * 		double 双线
				 * 
				 * style也可以分别指定四个边的边框样式，规则和width一致，
				 * 	同时它也提供border-xxx-style四个样式，来分别设置四个边
				 */
				/*border-style: double;*/
				border-style: solid dotted dashed double; 
			}
			
			
		</style>
	</head>
	<body>
		<div class="box1"></div>
	</body>
</html>

```

<br/>

<br/>

### 边框

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style type="text/css">
			
			.box{
				width: 200px;
				height: 200px;
				background-color: #bfa;
				
				/*设置边框
				 	大部分的浏览器中，边框的宽度和颜色都是有默认值，而边框的样式默认值都是none
				 * */
				/*border-width:10px ;
				border-color: red;
				border-style: solid;*/
				
				/*
				 * border
				 * 	- 边框的简写样式，通过它可以同时设置四个边框的样式，宽度，颜色
				 * 	- 而且没有任何的顺序要求
				 * 	- border一指定就是同时指定四个边不能分别指定
				 * 
				 * border-top border-right border-bottom border-left
				 * 	可以单独设置四个边的样式，规则和border一样，只不过它只对一个边生效
				 */
				/*border: red solid 10px   ;*/
				/*border-left: red solid 10px   ;*/
				
				/*border-top: red solid 10px;
				border-bottom: red solid 10px;
				border-left: red solid 10px;*/
				
				border: red solid 10px;
				border-right: none;
				
			}
			
		</style>
	</head>
	<body>
		
		<div class="box"></div>
		
	</body>
</html>

```

<br/>

<br/>

### 内边距

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 200px;
				height: 200px;
				background-color: #bfa;
				/*设置边框*/
				border: 10px red solid;
				
				/*
				 * 内边距（padding），指的是盒子的内容区与盒子边框之间的距离
				 * 	一共有四个方向的内边距，可以通过：
				 * 		padding-top
				 * 		padding-right
				 * 		padding-bottom
				 * 		padding-left
				 * 			来设置四个方向的内边距
				 * 
				 * 内边距会影响盒子的可见框的大小，元素的背景会延伸到内边距,
				 * 	盒子的大小由内容区、内边距和边框共同决定
				 * 	盒子可见框的宽度 = border-left-width + padding-left + width + padding-right + border-right-width
				 *  可见宽的高度 = border-top-width + padding-top + height + padding-bottom + border-bottom-width
				 */
				
				/*设置上内边距*/
				/*padding-top: 100px;*/
				/*设置右内边距*/
				/*padding-right: 100px;
				padding-bottom: 100px;
				padding-left: 100px;*/
				
				/*
				 * 使用padding可以同时设置四个边框的样式，规则和border-width一致
				 */
				/*padding: 100px;*/
				
				/*padding: 100px 200px;*/
				
				/*padding: 100px 200px 300px;*/
				
				padding: 100px 200px 300px 400px;
			}
			
			/*
			 * 创建一个子元素box1占满box2
			 */
			.box2{
				width: 100%;
				height: 100%;
				background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		
		<div class="box1">
			<div class="box2"></div>
		</div>
		
	</body>
</html>

```

<br/>

<br/>

### 外边距1

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 200px;
				height: 200px;
				background-color: #bfa;
				border: 10px solid red;
				/*
				 * 外边距指的是当前盒子与其他盒子之间的距离，
				 * 	他不会影响可见框的大小，而是会影响到盒子的位置。
				 * 盒子有四个方向的外边距：
				 * 	margin-top
				 * 	margin-right
				 * 	margin-bottom
				 * 	margin-left
				 * 
				 * 由于页面中的元素都是靠左靠上摆放的，
				 * 	所以注意当我们设置上和左外边距时，会导致盒子自身的位置发生改变，
				 * 	而如果是设置右和下外边距会改变其他盒子的位置
				 */
				/*
				 * 设置box1的上外边距，盒子上边框和其他的盒子的距离
				 */
				/*margin-top: 100px;*/
				
				/*
				 * 左外边距
				 */
				/*margin-left: 100px;*/
				
				/*设置右和下外边距*/
				/*margin-right: 100px;
				margin-bottom: 100px;*/
				
				/*
				 * 外边距也可以指定为一个负值，
				 * 	如果外边距设置的是负值，则元素会向反方向移动
				 */
				/*margin-left: -150px;
				margin-top: -100px;
				margin-bottom: -100px;*/
				/*margin-bottom: -100px;*/
				
				/*
				 * margin还可以设置为auto，auto一般只设置给水平方向的margin
				 * 	如果只指定，左外边距或右外边距的margin为auto则会将外边距设置为最大值
				 * 	垂直方向外边距如果设置为auto，则外边距默认就是0
				 * 
				 * 如果将left和right同时设置为auto，则会将两侧的外边距设置为相同的值，
				 * 	就可以使元素自动在父元素中居中，所以我们经常将左右外边距设置为auto
				 * 	以使子元素在父元素中水平居中
				 * 
				 */
				
				/*margin-left: auto;
				margin-right: auto;*/
				
				/*
				 * 外边距同样可以使用简写属性 margin，可以同时设置四个方向的外边距,
				 * 	规则和padding一样
				 */
				margin: 0 auto;
				
			}
			
			.box2{
				width: 200px;
				height: 200px;
				background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		<div class="box1"></div>
		<div class="box2"></div>
	</body>
</html>

```

<br/>

<br/>

### 外边距2

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 100px;
				height: 100px;
				background-color: red;
				/*
				 * 为上边的元素设置一个下外边距
				 */
				margin-bottom: 100px;
			}
			
			/*
			 * 垂直外边距的重叠
			 * 	- 在网页中相邻的垂直方向的外边距会发生外边距的重叠
			 * 		所谓的外边距重叠指兄弟元素之间的相邻外边距会取最大值而不是取和
			 * 		如果父子元素的垂直外边距相邻了，则子元素的外边距会设置给父元素
			 */
			
			.box2{
				width: 100px;
				height: 100px;
				background-color: green;
				/**
				 * 为下边的元素设置一个上外边距
				 */
				margin-top: 100px;
			}
			
			.box3{
				width: 200px;
				height: 100px;
				background-color: yellow;
				
				/*为box3设置一个上边框*/
				/*border-top: 1px red solid;*/
				/*padding-top: 1px;*/
				
				padding-top: 100px;
			}
			
			.box4{
				width: 100px;
				height: 100px;
				background-color: yellowgreen;
				/*
				 * 为子元素设置一个上外边距，是子元素的位置下移
				 */
				/*margin-top: 100px;*/
			}
			
		</style>
	</head>
	<body>
		
		<div class="box3">
			<div class="box4"></div>
		</div>
		
		<div class="box1"></div>
		<div class="box2"></div>
	</body>
</html>

```

<br/>

<br/>

### 浏览器默认样式

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			/*
			 * 浏览器为了在页面中没有样式时，也可以有一个比较好的显示效果，
			 * 	所以为很多的元素都设置了一些默认的margin和padding，
			 * 	而它的这些默认样式，正常情况下我们是不需要使用的。
			 * 
			 * 所以我们往往在编写样式之前需要将浏览器中的默认的margin和padding统统的去掉
			 * 	
			 */
			
			/*
			 * 清除浏览器的默认样式
			 */
			
			*{
				margin: 0;
				padding: 0;
			}
		
			
			.box1{
				width: 100px;
				height: 100px;
				background-color: #bfa;
			}
			
			p{
				background-color: yellow;
			}
			
			
		</style>
	</head>
	<body>
		<div class="box1"></div>
		
		<p>我是一个段落</p>
		<p>我是一个段落</p>
		<p>我是一个段落</p>
		
		<ul>
			<li>无序列表</li>
			<li>无序列表</li>
			<li>无序列表</li>
			<li>无序列表</li>
		</ul>
	</body>
</html>

```

<br/>

<br/>

### 内联元素的盒子

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			span{
				background-color: #bfa;
			}
			
			.box1{
				width: 100px;
				height: 100px;
				background-color: red;
			}
			
			.s1{
				/*
				 	内容区、内边距 、边框 、外边距
				 * */
				
				/*
				 * 内联元素不能设置width和height
				 */
				/*width: 200px;
				height: 200px;*/
				
				/*
				 * 设置水平内边距,内联元素可以设置水平方向的内边距
				 */
				padding-left: 100px ;
				padding-right: 100px ;
				
				/*
				 * 垂直方向内边距，内联元素可以设置垂直方向内边距，但是不会影响页面的布局
				 */
				/*padding-top: 50px;
				padding-bottom: 50px;*/
				
				/*
				 * 为元素设置边框,
				 * 	内联元素可以设置边框，但是垂直的边框不会影响到页面的布局
				 */
				border: 1px blue solid;
				
				/*
				 * 水平外边距
				 * 	内联元素支持水平方向的外边距
				 */
				margin-left:100px ;
				margin-right: 100px;
				
				/*
				 * 内联元素不支持垂直外边距
				 */
				/*margin-top: 200px;
				margin-bottom: 200px;*/
				
			}
			
			.s2{
				/*
				 * 为右边的元素设置一个左外边距
				 * 水平方向的相邻外边距不会重叠，而是求和
				 */
				margin-left: 100px;
			}
			
			
		</style>
	</head>
	<body>
		<span class="s1">我是一个span</span>
		<span class="s2">我是一个span</span>
		<span>我是一个span</span>
		<span>我是一个span</span>
		
		<div class="box1"></div>
	</body>
</html>

```

<br/>

<br/>

### display

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			a{
				background-color: #bfa;
				
				/*
				 * 将一个内联元素变成块元素，
				 * 	通过display样式可以修改元素的类型
				 * 		可选值：
				 * 			inline：可以将一个元素作为内联元素显示
				 * 			block: 可以将一个元素设置块元素显示
				 * 			inline-block：将一个元素转换为行内块元素
				 * 					- 可以使一个元素既有行内元素的特点又有块元素的特点
				 * 						既可以设置宽高，又不会独占一行
				 * 			none: 不显示元素，并且元素不会在页面中继续占有位置
				 */
				display: none;
				
				/*为其设置一个宽和高*/
				width: 500px;
				height: 500px;
			}
			
			.box{
				width: 100px;
				height: 100px;
				background-color: orange;
				
				/*
				 * display: none;
				 * 	使用该方式隐藏的元素，不会在页面中显示，并且不再占据页面的位置
				 */
				/*display: none;*/
				
				/*
				 * visibility
				 * 	- 可以用来设置元素的隐藏和显示的状态
				 * 	- 可选值：
				 * 		visible 默认值，元素默认会在页面显示
				 * 		hidden 元素会隐藏不显示	
				 * 
				 * 使用 visibility:hidden;隐藏的元素虽然不会在页面中显示，
				 * 		但是它的位置会依然保持
				 */
				visibility:hidden ;
			}
			
		</style>
	</head>
	<body>
		
		<div class="box"></div>
		
		<a href="#">我是一个大大的超链接</a>
		
		<span>hello</span>
		
	</body>
</html>

```

<br/>

<br/>

### overflow

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 200px;
				height: 200px;
				background-color: #bfa;
				/*
				 * 子元素默认是存在于父元素的内容区中，
				 * 		理论上讲子元素的最大可以等于父元素内容区大小
				 * 	如果子元素的大小超过了父元素的内容区，则超过的大小会在父元素以外的位置显示，
				 * 		超出父元素的内容，我们称为溢出的内容
				 *  父元素默认是将溢出内容，在父元素外边显示，
				 * 	通过overflow可以设置父元素如何处理溢出内容：
				 * 		可选值：
				 * 			- visible，默认值，不会对溢出内容做处理，元素会在父元素以外的位置显示
				 * 			- hidden, 溢出的内容，会被修剪，不会显示
				 * 			- scroll, 会为父元素添加滚动条，通过拖动滚动条来查看完整内容
				 * 					- 该属性不论内容是否溢出，都会添加水平和垂直双方向的滚动条
				 * 			- auto，会根据需求自动添加滚动条，
				 * 						需要水平就添加水平
				 * 						需要垂直就添加垂直
				 * 						都不需要就都不加
				 */
				overflow: auto;
			}
			
			.box2{
				width: 100px;
				height: 500px;
				background-color: red;
			}
			
		</style>
	</head>
	<body>
		
		<div class="box1">
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。
这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。
我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。
枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。
鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。
哇的一声，夜游的恶鸟飞过了。
我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。
后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。
猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。
我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。
一九二四年九月十五日。 
		</div>
		
	</body>
</html>

```

<br/>

<br/>

### 文档流

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		
		<!-- 
			文档流
				文档流处在网页的最底层，它表示的是一个页面中的位置，
				 我们所创建的元素默认都处在文档流中
				 
			元素在文档流中的特点
				块元素
					1.块元素在文档流中会独占一行，块元素会自上向下排列。
					2.块元素在文档流中默认宽度是父元素的100%
					3.块元素在文档流中的高度默认被内容撑开
				内联元素
					1.内联元素在文档流中只占自身的大小，会默认从左向右排列，
						如果一行中不足以容纳所有的内联元素，则换到下一行，
						继续自左向右。
					2.在文档流中，内联元素的宽度和高度默认都被内容撑开	
		-->
		
		<!-- 
			当元素的宽度的值为auto时，此时指定内边距不会影响可见框的大小，
				而是会自动修改宽度，以适应内边距
		-->
		<div style="background-color: #bfa;">
			<div style="height: 50px;"></div>
		</div>
		<div style="width: 100px; height: 100px; background-color: #ff0;"></div>
		
		
		<span style="background-color: yellowgreen;">我是一个span</span>
		<span style="background-color: yellowgreen;">我是一个span</span>
		<span style="background-color: yellowgreen;">我是一个span</span>
		<span style="background-color: yellowgreen;">我是一个span</span>
		<span style="background-color: yellowgreen;">我是一个span</span>
		<span style="background-color: yellowgreen;">我是一个span</span>
	</body>
</html>

```

<br/>

<br/>

### 浮动1

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 600px;
				height: 200px;
				background-color: red;
				/*
				 * 块元素在文档流中默认垂直排列，所以这个三个div自上至下依次排开，
				 * 	如果希望块元素在页面中水平排列，可以使块元素脱离文档流
				 * 使用float来使元素浮动，从而脱离文档流
				 * 	可选值：
				 * 		none，默认值，元素默认在文档流中排列
				 * 		left，元素会立即脱离文档流，向页面的左侧浮动
				 * 		right，元素会立即脱离文档流，向页面的右侧浮动
				 * 
				 * 当为一个元素设置浮动以后（float属性是一个非none的值），
				 * 	元素会立即脱离文档流，元素脱离文档流以后，它下边的元素会立即向上移动
				 * 	元素浮动以后，会尽量向页面的左上或这是右上漂浮，
				 * 	直到遇到父元素的边框或者其他的浮动元素
				 * 	如果浮动元素上边是一个没有浮动的块元素，则浮动元素不会超过块元素
				 * 	浮动的元素不会超过他上边的兄弟元素，最多最多一边齐
				 */
				float: left;
			}
			
			.box2{
				width: 600px;
				height: 200px;
				background-color: yellow;
				
				float: left;
			}
			
			.box3{
				width: 200px;
				height: 200px;
				background-color: green;
				
				float: right;
			}
			
			
		</style>
	</head>
	<body>
		
		<div class="box1"></div>
		<div class="box2"></div>
		<div class="box3"></div>
		
	</body>
</html>

```

<br/>

<br/>

### 浮动2

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
		
			*{
				margin: 0;
				padding: 0;
			}
			.box1{
				width: 100px;
				height: 100px;
				background-color: #bfa;
				/*
				 * 浮动的元素不会盖住文字，文字会自动环绕在浮动元素的周围，
				 * 	所以我们可以通过浮动来设置文字环绕图片的效果
				 */ 
				 
				/*box1向左浮动*/
				float: left;
				
			}
			
			.p1{
				background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		
		<div class="box1"></div>
		
		<p class="p1">
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。
这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。
我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。
枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。
鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。
哇的一声，夜游的恶鸟飞过了。
		</p>
		
	</body>
</html>

```

<br/>

<br/>

### 浮动3

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				/*
				 * 在文档流中，子元素的宽度默认占父元素的全部
				 */
				background-color: #bfa;
				
				/*
				 * 当元素设置浮动以后，会完全脱离文档流.
				 * 	块元素脱离文档流以后，高度和宽度都被内容撑开
				 */
				/*float: left;*/
			}
			
			.s1{
				/*
				 * 开启span的浮动
				 * 	内联元素脱离文档流以后会变成块元素
				 */
				float: left;
				width: 100px;
				height: 100px;
				background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		
		<div class="box1">a</div>
		
		<span class="s1">hello</span>
		
	</body>
</html>

```

<br/>

<br/>

### 简单布局

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			/*清除默认样式*/
			*{
				margin: 0;
				padding: 0;
			}
			
			/*设置头部div*/
			.header{
				/*设置一个宽度*/
				width: 1000px;
				/*设置一个高度*/
				height: 120px;
				/*设置一个背景颜色*/
				background-color: yellowgreen;
				/*设置居中*/
				margin: 0 auto;
			}
			
			/*设置一个content*/
			.content{
				/*设置一个宽度*/
				width: 1000px;
				/*设置一个高度*/
				height: 400px;
				/*设置一个背景颜色*/
				background-color: orange;
				/*居中*/
				margin: 10px auto;
			}
			
			/*设置content中小div的样式*/
			.left{
				width: 200px;
				height: 100%;
				background-color: skyblue;
				/*向左浮动*/
				float: left;
			}
			
			.center{
				width: 580px;
				height: 100%;
				background-color: yellow;
				/*向左浮动*/
				float: left;
				/*设置水平外边距*/
				margin: 0 10px;
			}
			
			.right{
				width: 200px;
				height: 100%;
				background-color: pink;
				/*向左浮动*/
				float: left;
			}
			
			
			
			/*设置一个footer*/
			.footer{
				/*设置一个宽度*/
				width: 1000px;
				/*设置一个高度*/
				height: 120px;
				/*设置一个背景颜色*/
				background-color: silver;
				/*居中*/
				margin: 0 auto;
			}
			
		</style>
	</head>
	<body>
		<!-- 头部div -->
		<div class="header"></div>
		
		<!-- 主体内容div -->
		<div class="content">
			
			<!-- 左侧div -->
			<div class="left"></div>
			<!-- 中间div -->
			<div class="center"></div>
			<!-- 右侧div -->
			<div class="right"></div>
			
		</div>
		
		<!-- 底部信息div -->
		<div class="footer"></div>
		
	</body>
</html>

```

<br/>

<br/>

### 高度塌陷

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style type="text/css">
			
			.box1{
				/*为box1设置一个边框*/
				border: 10px red solid;
				
			}
			
			.box2{
				width: 100px;
				height: 100px;
				background-color: blue;
				
				/*
				 * 在文档流中，父元素的高度默认是被子元素撑开的，
				 * 	也就是子元素多高，父元素就多高。
				 * 但是当为子元素设置浮动以后，子元素会完全脱离文档流，
				 * 	此时将会导致子元素无法撑起父元素的高度，导致父元素的高度塌陷。
				 * 	由于父元素的高度塌陷了，则父元素下的所有元素都会向上移动，这样将会导致页面布局混乱。
				 * 
				 * 所以在开发中一定要避免出现高度塌陷的问题,
				 * 	我们可以将父元素的高度写死，以避免塌陷的问题出现，
				 * 	但是一旦高度写死，父元素的高度将不能自动适应子元素的高度，所以这种方案是不推荐使用的。
				 */
				
				/*为子元素设置向左浮动*/
				float: left;
			}
			
			.box3{
				 height: 100px;
				 background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		
		<div class="box1">
			<div class="box2"></div>
		</div>
		
		<div class="box3"></div>
		
	</body>
</html>

```

<br/>

<br/>

### 解决高度塌陷

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style type="text/css">
			
			.box1{
				/*为box1设置一个边框*/
				border: 10px red solid;
				
			}
			
			.box2{
				width: 100px;
				height: 100px;
				background-color: blue;
				
				/*
				 * 在文档流中，父元素的高度默认是被子元素撑开的，
				 * 	也就是子元素多高，父元素就多高。
				 * 但是当为子元素设置浮动以后，子元素会完全脱离文档流，
				 * 	此时将会导致子元素无法撑起父元素的高度，导致父元素的高度塌陷。
				 * 	由于父元素的高度塌陷了，则父元素下的所有元素都会向上移动，这样将会导致页面布局混乱。
				 * 
				 * 所以在开发中一定要避免出现高度塌陷的问题,
				 * 	我们可以将父元素的高度写死，以避免塌陷的问题出现，
				 * 	但是一旦高度写死，父元素的高度将不能自动适应子元素的高度，所以这种方案是不推荐使用的。
				 */
				
				/*为子元素设置向左浮动*/
				float: left;
			}
			
			.box3{
				 height: 100px;
				 background-color: yellow;
			}
			
		</style>
	</head>
	<body>
		
		<div class="box1">
			<div class="box2"></div>
		</div>
		
		<div class="box3"></div>
		
	</body>
</html>

```

<br/>

<br/>

### 导航条

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>导航条</title>
		<style type="text/css">
			
			/*
			 * 清除默认样式
			 */
			*{
				margin:0;
				padding:0;
			}
			
			/*
			 * 设置ul
			 */
			.nav{
				/*去除项目符号*/
				list-style: none;
				/*为ul设置一个背景颜色*/
				background-color: #6495ED;
				/*设置一个宽度*/
				/*
				 * 在IE6中，如果为元素指定了一个宽度，则会默认开启hasLayout
				 */
				width: 1000px;
				/*设置元素居中*/
				margin: 50px auto;
				/*解决高度塌陷*/
				overflow: hidden;
			}
			
			/**
			 * 设置li
			 */
			.nav li{
				/*设置li向左浮动*/
				float: left;
				width: 12.5%;
			}
			
			.nav a{
				/*将a转换为块元素*/
				display: block;
				/*为a指定一个宽度*/
				width: 100%;
				/*设置文字居中*/
				text-align: center;
				/*设置一个上下内边距*/
				padding: 5px 0;
				/*去除下划线*/
				text-decoration: none;
				/*设置字体颜色*/
				color: white;
				/*设置加粗*/
				font-weight: bold;
			}
			
			/*
			 * 设置a的鼠标移入的效果
			 */
			.nav a:hover{
				background-color: #c00;
			}
			
			
		</style>
	</head>
	<body>
		
		<!-- 创建导航条的结构 -->
		<ul class="nav">
			<li><a href="#">首页</a></li>
			<li><a href="#">新闻</a></li>
			<li><a href="#">联系</a></li>
			<li><a href="#">关于</a></li>
			<li><a href="#">首页</a></li>
			<li><a href="#">新闻</a></li>
			<li><a href="#">联系</a></li>
			<li><a href="#">关于</a></li>
		</ul>
		
	</body>
</html>

```

<br/>

<br/>

### 清除浮动

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		
		<style type="text/css">
			
			.box1{
				width: 100px;
				height: 100px;
				background-color: yellow;
				/*
				 * 设置box1向左浮动
				 */
				float: left;
			}
			.box2{
				width: 200px;
				height: 200px;
				background-color: yellowgreen;
				/*
				 * 由于受到box1浮动的影响，box2整体向上移动了100px
				 * 我们有时希望清除掉其他元素浮动对当前元素产生的影响，这时可以使用clear来完成功能
				 * clear可以用来清除其他浮动元素对当前元素的影响
				 * 可选值：
				 * 		none，默认值，不清除浮动
				 * 		left，清除左侧浮动元素对当前元素的影响
				 * 		right，清除右侧浮动元素对当前元素的影响
				 * 		both，清除两侧浮动元素对当前元素的影响
				 * 				清除对他影响最大的那个元素的浮动
				 */
				
				/*
				 * 清除box1浮动对box2产生的影响
				 * 清除浮动以后，元素会回到其他元素浮动之前的位置
				 */
				/*clear: left;*/
				float: right;
			}
			.box3{
				width: 300px;
				height: 300px;
				background-color: skyblue;
				
				clear: both;
			}
			
		</style>
		
	</head>
	<body>
		
		<div class="box1"></div>
		
		<div class="box2"></div>
		
		<div class="box3"></div>
		
	</body>
</html>

```

<br/>

<br/>

### 解决高度塌陷2

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				border: 1px solid red;
			}
			
			.box2{
				width: 100px;
				height: 100px;
				background-color: blue;
				
				float: left;
			}
			
			/*
			 * 解决高度塌陷方案二：
			 * 	可以直接在高度塌陷的父元素的最后，添加一个空白的div，
			 * 	由于这个div并没有浮动，所以他是可以撑开父元素的高度的，
			 * 	然后在对其进行清除浮动，这样可以通过这个空白的div来撑开父元素的高度，
			 * 	基本没有副作用
			 * 
			 * 使用这种方式虽然可以解决问题，但是会在页面中添加多余的结构。
			 */
			.clear{
				clear: both;
			}
			
		</style>
	</head>
	<body>
		<div class="box1">
			<div class="box2"></div>
			<div class="clear"></div>
		</div>
	</body>
</html>

```

<br/>

<br/>

### 解决高度塌陷3

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				border: 1px solid red;
			}
			
			.box2{
				width: 100px;
				height: 100px;
				background-color: blue;
				
				float: left;
			}
			
			/*通过after伪类，选中box1的后边*/
			/*
			 * 可以通过after伪类向元素的最后添加一个空白的块元素，然后对其清除浮动，
			 * 	这样做和添加一个div的原理一样，可以达到一个相同的效果，
			 * 	而且不会在页面中添加多余的div，这是我们最推荐使用的方式，几乎没有副作用
			 */
			.clearfix:after{
				/*添加一个内容*/
				content: "";
				/*转换为一个块元素*/
				display: block;
				/*清除两侧的浮动*/
				clear: both;
			}
			
			/*
			 * 在IE6中不支持after伪类,
			 * 	所以在IE6中还需要使用hasLayout来处理
			 */
			.clearfix{
				zoom:1;
			}
			
			
		</style>
	</head>
	<body>
		<div class="box1">
			<div class="box2"></div>
		</div>
	</body>
</html>

```

<br/>

<br/>

### 开班信息

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			/*清除默认样式*/
			*{
				margin: 0;
				padding: 0;
			}
			
			/*统一页面中的字体*/
			body{
				font: 12px/1 宋体;
			}
			
			/*设置outer的大小*/
			.outer{
				width: 300px;
				/*height: 473px;*/
				/*background-color: #bfa;*/
				/*设置outer居中效果*/
				margin: 50px auto;
			}
			
			/*
			 * 设置title的边框
			 */
			.title{
				/*设置上边框*/
				border-top:2px #019e8b solid ;
				/*设置盒子的高度*/
				height: 36px;
				/*设置背景样式*/
				background-color: #f5f5f5;
				/*设置title的行高*/
				line-height: 36px;
				/*设置title的内边距*/
				padding: 0px 22px 0px 16px;
			}
			
			/*
			 	设置title中的超链接
			 * */
			.title a{
				float: right;
				/*设置字体颜色*/
				color: red;
			}
			
			/*设置h3*/
			.title h3{
				font: 16px/36px "微软雅黑";
			}
			
			/*设置内容*/
			.content{
				border: 1px solid #deddd9;
				/*设置内边距*/
				padding: 0px 28px 0px 20px;
			}
			
			/*设置内容中的超链接*/
			.content a{
				color: black;
				/*去除超链接的下划线*/
				text-decoration: none;
				/*设置字体大小*/
				font-size: 12px;
			}
			
			/*为超链接添加一个hover伪类*/
			.content a:hover{
				color: red;
				/*为超链接添加下划线*/
				text-decoration: underline;
			}
			
			/*设置内容中的标题*/
			.content h3{
				margin-top: 14px;
				margin-bottom: 16px;
			}
			
			/*
			 * 设置右侧的a的样式
			 */
			.content .right{
				/*设置向右浮动*/
				float: right;
			}
			
			/*设置ul的样式*/
			.content ul{
				/*去除项目符号*/
				list-style: none;
				/*为ul设置一个下边框*/
				border-bottom: 1px dashed #deddd9;
			}
			
			/*取消最后一个ul的边框*/
			.content .no-border{
				border: none;
			}
			
			/*设置内容中的红色字体*/
			.content .red{
				color: red;
				font-weight: bold;
			}
			
			/*设置内容中的li*/
			.content li{
				margin-bottom: 15px;
			}
			
			
		</style>
	</head>
	<body>
		
		<!--创建一个外层div，容纳整个内容-->
		<div class="outer">
			
			<!-- 开班信息的头部 -->
			<div class="title">
				<a href="#">16年面授开班计划</a>
				<h3>近期开班</h3>
			</div>
			
			<!-- 开班信息的主要内容 -->
			<div class="content">
				<h3><a href="#">JavaEE+云计算-全程就业班</a></h3>
				<ul>
					<li>
						<a class="right" href="#"><span class="red">预约报名</span></a>
						<a href="#">开班时间：<span class="red">2016-04-27</span></a>
					</li>
					<li>
						<a class="right" href="#"><span class="red">无座，名额爆满</span></a>
						<a href="#">开班时间：<span class="red">2016-04-07</span></a>
					</li>
					<li>
						<a class="right" href="#"><span>开班盛况</span></a>
						<a href="#">开班时间：<span>2016-03-15</span></a>
					</li>
					<li>
						<a class="right" href="#"><span>开班盛况</span></a>
						<a href="#">开班时间：<span>2016-02-25</span></a>
					</li>
					<li>
						<a class="right" href="#"><span>开班盛况</span></a>
						<a href="#">开班时间：<span>2015-12-26</span></a>
					</li>
				</ul>
				
				<h3><a href="#">Android+人工智能-全程就业班</a></h3>
				<ul>
					<li>
						<a class="right" href="#"><span class="red">预约报名</span></a>
						<a href="#">开班时间：<span class="red">2016-04-10</span></a>
					</li>
					<li>
						<a class="right" href="#"><span>开班盛况</span></a>
						<a href="#">开班时间：<span>2016-03-17</span></a>
					</li>
					<li>
						<a class="right" href="#"><span>开班盛况</span></a>
						<a href="#">开班时间：<span>2016-02-20</span></a>
					</li>
					<li>
						<a class="right" href="#"><span>开班盛况</span></a>
						<a href="#">开班时间：<span>2015-12-23</span></a>
					</li>
				</ul>
				<h3><a href="#">前端+HTML5-全程就业班</a></h3>
				<ul class="no-border">
					<li>
						<a class="right" href="#"><span class="red">预约报名</span></a>
						<a href="#">开班时间：<span class="red">2016-05-10</span></a>
					</li>
					<li>
						<a class="right" href="#"><span>开班盛况</span></a>
						<a href="#">开班时间：<span>2016-03-16</span></a>
					</li>
				</ul>
			</div>
			
		</div>
		
	</body>
</html>

```

<br/>

<br/>

### 相对定位

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		
		<style type="text/css">
			
			.box1{
				width: 200px;
				height: 200px;
				background-color: red;
			}
			
			.box2{
				width: 200px;
				height: 200px;
				background-color: yellow;
				/*
				 * 定位：
				 * 	- 定位指的就是将指定的元素摆放到页面的任意位置
				 * 		通过定位可以任意的摆放元素
				 * 	- 通过position属性来设置元素的定位
				 * 		-可选值：
				 * 			static：默认值，元素没有开启定位
				 * 			relative：开启元素的相对定位
				 * 			absolute：开启元素的绝对定位
				 * 			fixed：开启元素的固定定位（也是绝对定位的一种）
				 */
				
				/*
				 * 当元素的position属性设置为relative时，则开启了元素的相对定位
				 * 	1.当开启了元素的相对定位以后，而不设置偏移量时，元素不会发生任何变化
				 *  2.相对定位是相对于元素在文档流中原来的位置进行定位
				 * 	3.相对定位的元素不会脱离文档流
				 * 	4.相对定位会使元素提升一个层级
				 * 	5.相对定位不会改变元素的性质，块还是块，内联还是内联
				 */
				position: relative;
				
				
				/*
				 * 当开启了元素的定位（position属性值是一个非static的值）时，
				 * 	可以通过left right top bottom四个属性来设置元素的偏移量
				 * 	left：元素相对于其定位位置的左侧偏移量
				 * 	right：元素相对于其定位位置的右侧偏移量
				 * 	top：元素相对于其定位位置的上边的偏移量
				 * 	bottom：元素相对于其定位位置下边的偏移量
				 * 
				 * 通常偏移量只需要使用两个就可以对一个元素进行定位，
				 * 	一般选择水平方向的一个偏移量和垂直方向的偏移量来为一个元素进行定位
				 */
				
				/*left: 100px;
				top: 200px;*/
				
				left: 200px;
				
			}
			
			.box3{
				width: 200px;
				height: 200px;
				background-color: yellowgreen;
				
			}
			
			.s1{
				position: relative;
				width: 200px;
				height: 200px;
				background-color: yellow;
			}
			
		</style>
		
	</head>
	<body>
		
		<div class="box1"></div>
		<div class="box2"></div>
		<div class="box3"></div>
		
		<span class="s1">我是一个span</span>
		
	</body>
</html>

```

<br/>

<br/>

### 绝对定位

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		
		<style type="text/css">
			
			.box1{
				width: 200px;
				height: 200px;
				background-color: red;
			}
			.box2{
				width: 200px;
				height: 200px;
				background-color: yellow;
				/*
				 * 当position属性值设置为absolute时，则开启了元素的绝对定位
				 * 
				 * 绝对定位：
				 * 	1.开启绝对定位，会使元素脱离文档流
				 * 	2.开启绝对定位以后，如果不设置偏移量，则元素的位置不会发生变化
				 * 	3.绝对定位是相对于离他最近的开启了定位的祖先元素进行定位的（一般情况，开启了子元素的绝对定位都会同时开启父元素的相对定位）
				 * 		如果所有的祖先元素都没有开启定位，则会相对于浏览器窗口进行定位
				 * 	4.绝对定位会使元素提升一个层级
				 * 	5.绝对定位会改变元素的性质，
				 * 		内联元素变成块元素，
				 * 		块元素的宽度和高度默认都被内容撑开
				 */
				position: absolute;
				
				/*left: 100px;
				top: 100px;*/
				
			}
			.box3{
				width: 300px;
				height: 300px;
				background-color: yellowgreen;
			}
			
			.box4{
				width: 300px;
				height: 300px;
				background-color: orange;
				/*开启box4的相对定位*/
				/*position: relative;*/
			}
			
			.s1{
				width: 100px;
				height: 100px;
				background-color: yellow;
				
				/*开启绝对定位*/
				position: absolute;
			}
			
		</style>
		
	</head>
	<body>
		
		<div class="box1"></div>
		<div class="box5">
			<div class="box4">
				<div class="box2"></div>
			</div>
		</div>
		
		<div class="box3"></div>
		
		<span class="s1">我是一个span</span>
	</body>
</html>

```

<br/>

<br/>

### 固定定位

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		
		<style type="text/css">
			
			.box1{
				width: 200px;
				height: 200px;
				background-color: red;
			}
			.box2{
				width: 200px;
				height: 200px;
				background-color: yellow;
				/*
				 * 当元素的position属性设置fixed时，则开启了元素的固定定位
				 * 	固定定位也是一种绝对定位，它的大部分特点都和绝对定位一样
				 * 不同的是：
				 * 		固定定位永远都会相对于浏览器窗口进行定位
				 * 		固定定位会固定在浏览器窗口某个位置，不会随滚动条滚动
				 * 
				 * IE6不支持固定定位
				 */
				position: fixed;
				
				left: 0px;
				top: 0px;
				
			}
			.box3{
				width: 200px;
				height: 200px;
				background-color: yellowgreen;
			}
			
		</style>
		
	</head>
	<body style="height: 5000px;">
		
		<div class="box1"></div>
		<div class="box4" style="width: 300px; height: 300px; background-color: orange; position: relative;">
			<div class="box2"></div>
		</div>
		
		<div class="box3"></div>
	</body>
</html>

```

<br/>

<br/>

### 层级

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		
		<style type="text/css">
			
			.box1{
				width: 200px;
				height: 200px;
				background-color: red;
				position: relative;
				
				z-index: 2;
				
				opacity: 0.5;
				filter: alpha(opacity=50);
			}
			.box2{
				width: 200px;
				height: 200px;
				background-color: yellow;
				/*开启绝对定位*/
				position: absolute;
				/*设置偏移量*/				
				top: 100px;
				left: 100px;
				/*
				 * 如果定位元素的层级是一样，则下边的元素会盖住上边的
				 * 通过z-index属性可以用来设置元素的层级
				 * 可以为z-index指定一个正整数作为值，该值将会作为当前元素的层级
				 * 	层级越高，越优先显示
				 * 
				 * 对于没有开启定位的元素不能使用z-index
				 */
				z-index: 25;
				
				opacity: 0.5;
				filter: alpha(opacity=50);
				
			}
			.box3{
				width: 200px;
				height: 200px;
				background-color: yellowgreen;
				/*position: relative;
				z-index: 3;*/
				position: absolute;
				top: 200px;
				left: 200px;
				z-index: 30;
				
				/*
				 * 设置元素的透明背景
				 * opacity可以用来设置元素背景的透明，
				 * 	它需要一个0-1之间的值
				 * 		0 表示完全透明
				 * 		1 表示完全不透明
				 * 		0.5 表示半透明
				 */
				opacity: 0.5;
				
				/*
				 * opacity属性在IE8及以下的浏览器中不支持
				 * IE8及以下的浏览器需要使用如下属性代替
				 * 	alpha(opacity=透明度)
				 * 透明度，需要一个0-100之间的值
				 * 	0 表示完全透明
				 * 	100 表示完全不透明
				 * 	50 半透明
				 * 
				 * 这种方式支持IE6，但是这种效果在IE Tester中无法测试
				 */
				filter: alpha(opacity=50);
			}
			
			.box4{
				width: 200px;
				height: 200px;
				background-color: orange;
				/*开启相对定位*/
				position: relative;
				/*
				 * 父元素的层级再高，也不会盖住子元素
				 */
				z-index: 20;
				
				top: 500px;
			}
			
			.box5{
				width: 100px;
				height: 100px;
				background-color: skyblue;
				/*开启绝对定位*/
				position: absolute;
				z-index: 10;
			}
			
		</style>
		
	</head>
	<body style="height: 5000px;">
		
		<div class="box1"></div>
		<div class="box2"></div>
		<div class="box3"></div>
		<div class="box4">
			<div class="box5"></div>
		</div>
	</body>
</html>

```

<br/>

<br/>

### 背景

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 1024px;
				height: 724px;
				margin: 0 auto;
				/*设置背景样式*/
				background-color: #bfa;
				/*
				 * 使用background-image来设置背景图片
				 * 	- 语法：background-image:url(相对路径);
				 * 
				 * 	- 如果背景图片大于元素，默认会显示图片的左上角
				 * 	- 如果背景图片和元素一样大，则会将背景图片全部显示
				 * 	- 如果背景图片小于元素大小，则会默认将背景图片平铺以充满元素
				 * 
				 * 可以同时为一个元素指定背景颜色和背景图片，
				 * 	这样背景颜色将会作为背景图片的底色
				 * 	一般情况下设置背景图片时都会同时指定一个背景颜色
				 */
				background-image:url(img/1.png);
				
				/*
				 * background-repeat用于设置背景图片的重复方式
				 * 	可选值：
				 * 		repeat，默认值，背景图片会双方向重复（平铺）
				 * 		no-repeat ，背景图片不会重复，有多大就显示多大
				 * 		repeat-x， 背景图片沿水平方向重复
				 * 		repeat-y，背景图片沿垂直方向重复
				 */
				background-repeat: repeat-y;
			}
			
		</style>
		
		<!--<link rel="stylesheet" type="text/css" href="css/style.css"/>-->
	</head>
	<body>
		
		<div class="box1"></div>
		
	</body>
</html>

```

<br/>

<br/>

### 导航条

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		
		<style type="text/css">
			.box1{
				width: 990px;
				height: 32px;
				background-color: #bfa;
				margin: 50px auto;
				/*
				 * 设置为背景图片
				 */
				background-image: url(img/bg.gif);
				/*
				 * 设置水平方向重复
				 */
				background-repeat: repeat-x;
			}
		</style>
		
	</head>
	<body>
		<div class="box1"></div>
	</body>
</html>

```

<br/>

<br/>

### 背景2

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
		
			*{
				margin: 0;
				padding: 0;
			}
			
			.box1{
				height: 500px;
				margin:  0 auto;
				
				/*
				 * 设置一个背景颜色
				 */
				background-color: #bfa;
				/*
				 * 设置一个背景图片
				 */
				background-image: url(img/4.png);
				/*
				 * 设置一个图片不重复
				 */
				background-repeat: no-repeat;
				/*
				 * 背景图片默认是贴着元素的左上角显示
				 * 通过background-position可以调整背景图片在元素中的位置
				 * 可选值：
				 * 		该属性可以使用 top right left bottom center中的两个值
				 * 			来指定一个背景图片的位置
				 * 			top left 左上
				 * 			bottom right 右下
				 * 			如果只给出一个值，则第二个值默认是center
				 * 
				 * 		也可以直接指定两个偏移量，
				 * 			第一个值是水平偏移量
				 * 				- 如果指定的是一个正值，则图片会向右移动指定的像素
				 * 				- 如果指定的是一个负值，则图片会向左移动指定的像素
				 * 			第二个是垂直偏移量	
				 * 				- 如果指定的是一个正值，则图片会向下移动指定的像素
				 * 				- 如果指定的是一个负值，则图片会向上移动指定的像素
				 * 		
				 */
				/*background-position: -80px -40px;*/
				background-attachment: fixed;
			}
			
			body{
				background-image: url(img/3.png);
				background-repeat: no-repeat;
				
				/*
				 * background-attachment用来设置背景图片是否随页面一起滚动
				 * 		可选值：
				 * 			scroll，默认值，背景图片随着窗口滚动
				 * 			fixed，背景图片会固定在某一位置，不随页面滚动
				 * 
				 * 不随窗口滚动的图片，我们一般都是设置给body，而不设置给其他元素
				 */
				background-attachment:fixed ;
			}
			
			
		</style>
	</head>
	<body style="height: 5000px;">
		
		<div class="box1"></div>
		
	</body>
</html>

```

<br/>

<br/>

### 背景3

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			body {
				background-image: url(img/2.jpg);
				background-repeat: no-repeat;
				background-position: center;
				/*
				 * 当背景图片的background-attachment设置为fixed时，
				 * 	背景图片的定位永远相对于浏览器的窗口
				 */
				background-attachment: fixed;
			}
		</style>
	</head>

	<body style="height: 5000px;">

		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>
		<p>
			在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，我生平没有见过这样奇怪而高的天空。他仿佛要离开人间而去，使人们仰面不再看见。然而现在却非常之蓝，闪闪地䀹着几十个星星的眼，冷眼。他的口角上现出微笑，似乎自以为大有深意，而将繁霜洒在我的园里的野花草上。 我不知道那些花草真叫什么名字，人们叫他们什么名字。我记得有一种开过极细小的粉红花，现在还开着，但是更极细小了，她在冷的夜气中，瑟缩地做梦，梦见春的到来，梦见秋的到来，梦见瘦的诗人将眼泪擦在她最末的花瓣上，告诉她秋虽然来，冬虽然来，而此后接着还是春，蝴蝶乱飞，蜜蜂都唱起春词来了。她于是一笑，虽然颜色冻得红惨惨地，仍然瑟缩着。 枣树，他们简直落尽了叶子。先前，还有一两个孩子来打他们，别人打剩的枣子，现在是一个也不剩了，连叶子也落尽了。他知道小粉红花的梦，秋后要有春；他也知道落叶的梦，春后还是秋。他简直落尽叶子，单剩干子，然而脱了当初满树是果实和叶子时候的弧形，欠伸得很舒服。但是，有几枝还低亚着，护定他从打枣的竿梢所得的皮伤，而最直最长的几枝，却已默默地铁似的直刺着奇怪而高的天空，使天空闪闪地鬼䀹眼；直刺着天空中圆满的月亮，使月亮窘得发白。 鬼䀹眼的天空越加非常之蓝，不安了，仿佛想离去人间，避开枣树，只将月亮剩下。然而月亮也暗暗地躲到东边去了。而一无所有的干子，却仍然默默地铁似的直刺着奇怪而高的天空，一意要制他的死命，不管他各式各样地䀹着许多蛊惑的眼睛。 哇的一声，夜游的恶鸟飞过了。 我忽而听到夜半的笑声，吃吃地，似乎不愿意惊动睡着的人，然而四围的空气都应和着笑。夜半，没有别的人，我即刻听出这声音就在我嘴里，我也即刻被这笑声所驱逐，回进自己的房。灯火的带子也即刻被我旋高了。 后窗的玻璃上丁丁地响，还有许多小飞虫乱撞。不多久，几个进来了，许是从窗纸的破孔进来的。他们一进来，又在玻璃的灯罩上撞得丁丁地响。一个从上面撞进去了，他于是遇到火，而且我以为这火是真的。两三个却休息在灯的纸罩上喘气。那罩是昨晚新换的罩，雪白的纸，折出波浪纹的叠痕，一角还画出一枝猩红色的栀子。 猩红的栀子开花时，枣树又要做小粉红花的梦，青葱地弯成弧形了……我又听到夜半的笑声；我赶紧砍断我的心绪，看那老在白纸罩上的小青虫，头大尾小，向日葵子似的，只有半粒小麦那么大，遍身的颜色苍翠得可爱，可怜。 我打一个呵欠，点起一支纸烟，喷出烟来，对着灯默默地敬奠这些苍翠精致的英雄们。 一九二四年九月十五日。
		</p>

	</body>

</html>
```

<br/>

<br/>

### 练习

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
		
		/*
		 * 做完功能以后，发现在第一次切换图片时，会发现图片有一个非常快的闪烁，
		 * 	这个闪烁会造成一次不佳的用户体验。
		 * 产生问题的原因：
		 * 	背景图片是以外部资源的形式加载进网页的，浏览器每加载一个外部资源就需要单独的发送一次请求，
		 * 	但是我们外部资源并不是同时加载，浏览器会在资源被使用才去加载资源
		 * 	我们这个练习，一上来浏览器只会加载link.png由于hover和active的状态没有马上触发，
		 * 		所以hover.png和active.png并不是立即加载的
		 * 	当hover被触发时，浏览器才去加载hover.png
		 * 	当active被触发时，浏览器才去加载active.png
		 * 	由于加载图片需要一定的时间，所以在加载和显示过程会有一段时间，背景图片无法显示，导致出现闪烁的情况
		 * 
		 * 	为了解决该问题，可以将三个图片整合为一张图片，这样可以同时将三张图片一起加载，就不会出现闪烁的问题了，
		 * 	然后在通过background-position来切换要显示的图片的位置，这种技术叫做图片整合技术（CSS-Sprite）
		 * 	优点：
		 * 		1 将多个图片整合为一张图片里，浏览器只需要发送一次请求，可以同时加载多个图片，
		 * 			提高访问效率，提高了用户体验。
		 * 		2 将多个图片整合为一张图片，减小了图片的总大小，提高请求的速度，增加了用户体验
		 * 
		 * 
		 */
			
			.btn:link{
				/*将a转换为块元素*/
				display: block;
				/*设置宽高*/
				width: 93px;
				height: 29px;
				/*设置背景图片*/
				background-image: url(img/btn/btn2.png);
				/*设置背景图片不重复*/
				background-repeat: no-repeat;
				
			}
			
			.btn:hover{
				
				/*
				 * 当是hover状态时，希望图片可以向左移动
				 */
				background-position: -93px 0px;
			}
			
			.btn:active{
				/*
				 * 当是active状态时，希望图片再向左移动
				 */
				background-position: -186px 0px;
				
			}
			
		</style>
	</head>
	<body>
		
		<!-- 创建一个超链接 -->
		<a class="btn" href="#"></a>
		
	</body>
</html>

```

<br/>

<br/>

### 雪碧图(精灵图)

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 129px;
				height: 45px;
				background-image:url(img/amazon-sprite_.png) ;
			}
			
			.box2{
				width: 42px;
				height: 30px;
				background-image:url(img/amazon-sprite_.png) ;
				/*
				 * 设置偏移量
				 */
				background-position: -58px -338px;
			}
			
		</style>
	</head>
	<body>
		<div class="box1"></div>
		<div class="box2"></div>
	</body>
</html>

```

<br/>

<br/>

### 背景4

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			body{
				height: 5000px;
				/*设置一个背景颜色*/
				/*background-color: #bfa;*/
				/*设置一个背景图片*/
				/*background-image: url(img/3.png);*/
				/*设置背景不重复*/
				/*background-repeat: no-repeat;*/
				/*设置背景图片的位置*/
				/*background-position: center center;*/
				/*设置背景图片不随滚动条滚动*/ 
				/*background-attachment: fixed;*/
				
				background-color: #bfa;
				
				/*
				 * background
				 * 	- 通过该属性可以同时设置所有背景相关的样式
				 * 	- 没有顺序的要求，谁在前睡在后都行
				 * 		也没有数量的要求，不写的样式就使用默认值
				 */
				background: #bfa url(img/3.png) center center no-repeat fixed;
			}
			
		</style>
	</head>
	<body>
	</body>
</html>

```

<br/>

<br/>

### 表格1

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<!-- 
			表格在日常生活中使用的非常的多，比如excel就是专门用来创建表格的工具，
				表格就是用来表示一些格式化的数据的，比如：课程表、银行对账单
			在网页中也可以来创建出不同的表格。	
		-->
		
		<!--
			在HTML中，使用table标签来创建一个表格
		-->
		<table border="1" width="40%" align="center">
			
			<!-- 
				在table标签中使用tr来表示表格中的一行，有几行就有几个tr
			-->
			<tr>
				<!-- 在tr中需要使用td来创建一个单元格，有几个单元格就有几个td -->
				<td>A1</td>
				<td>A2</td>
				<td>A3</td>
				<td>A4</td>
			</tr>
			
			<tr>
				<td>B1</td>
				<td>B2</td>
				<td>B3</td>
				
				<!-- 
					rowspan用来设置纵向的合并单元格
				-->
				<td rowspan="2">B4</td>
			</tr>
			<tr>
				<td>C1</td>
				<td>C2</td>
				<td>C3</td>
			</tr>
			<tr>
				<td>D1</td>
				<td>D2</td>
				<!-- 
					colspan横向的合并单元格
				-->
				<td colspan="2">D3</td>
			</tr>
			
		</table>
		
	</body>
</html>

```

<br/>

<br/>

### 表格2

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			/*
			 * 设置表格的宽度
			 */
			table{
				width: 300px;
				/*居中*/
				margin: 0 auto;
				/*边框*/
				/*border:1px solid black;*/
				/*
				 * table和td边框之间默认有一个距离
				 * 	通过border-spacing属性可以设置这个距离
				 */
				/*border-spacing:0px ;*/
				
				/*
				 * border-collapse可以用来设置表格的边框合并
				 * 如果设置了边框合并，则border-spacing自动失效
				 */
				border-collapse: collapse;
				/*设置背景样式*/
				/*background-color: #bfa;*/
			}
			
			/*
			 * 设置边框
			 */
			td , th{
				border: 1px solid black;
			}
			
			/*
			 * 设置隔行变色
			 */
			tr:nth-child(even){
				background-color: #bfa;
			}
			
			/*
			 * 鼠标移入到tr以后，改变颜色
			 */
			tr:hover{
				background-color: #ff0;
			}
			
			
		</style>
	</head>
	<body>
		<!--
			table是一个块元素
		-->
		
		<table>
			<tr>
				<!--
					可以使用th标签来表示表头中的内容，
						它的用法和td一样，不同的是它会有一些默认效果
				-->
				<th>学号</th>
				<th>姓名</th>
				<th>性别</th>
				<th>住址</th>
			</tr>
			<tr>
				<td>1</td>
				<td>孙悟空</td>
				<td>男</td>
				<td>花果山</td>
			</tr>
			<tr>
				<td>2</td>
				<td>猪八戒</td>
				<td>男</td>
				<td>高老庄</td>
			</tr>
			<tr>
				<td>3</td>
				<td>沙和尚</td>
				<td>男</td>
				<td>流沙河</td>
			</tr>
			<tr>
				<td>4</td>
				<td>唐僧</td>
				<td>男</td>
				<td>女儿国</td>
			</tr>
			<tr>
				<td>1</td>
				<td>孙悟空</td>
				<td>男</td>
				<td>花果山</td>
			</tr>
			<tr>
				<td>2</td>
				<td>猪八戒</td>
				<td>男</td>
				<td>高老庄</td>
			</tr>
			<tr>
				<td>3</td>
				<td>沙和尚</td>
				<td>男</td>
				<td>流沙河</td>
			</tr>
			<tr>
				<td>4</td>
				<td>唐僧</td>
				<td>男</td>
				<td>女儿国</td>
			</tr>
			<tr>
				<td>1</td>
				<td>孙悟空</td>
				<td>男</td>
				<td>花果山</td>
			</tr>
			<tr>
				<td>2</td>
				<td>猪八戒</td>
				<td>男</td>
				<td>高老庄</td>
			</tr>
			<tr>
				<td>3</td>
				<td>沙和尚</td>
				<td>男</td>
				<td>流沙河</td>
			</tr>
			<tr>
				<td>4</td>
				<td>唐僧</td>
				<td>男</td>
				<td>女儿国</td>
			</tr>
			<tr>
				<td>1</td>
				<td>孙悟空</td>
				<td>男</td>
				<td>花果山</td>
			</tr>
			<tr>
				<td>2</td>
				<td>猪八戒</td>
				<td>男</td>
				<td>高老庄</td>
			</tr>
			<tr>
				<td>3</td>
				<td>沙和尚</td>
				<td>男</td>
				<td>流沙河</td>
			</tr>
			<tr>
				<td>4</td>
				<td>唐僧</td>
				<td>男</td>
				<td>女儿国</td>
			</tr>
			<tr>
				<td>1</td>
				<td>孙悟空</td>
				<td>男</td>
				<td>花果山</td>
			</tr>
			<tr>
				<td>2</td>
				<td>猪八戒</td>
				<td>男</td>
				<td>高老庄</td>
			</tr>
			<tr>
				<td>3</td>
				<td>沙和尚</td>
				<td>男</td>
				<td>流沙河</td>
			</tr>
			<tr>
				<td>4</td>
				<td>唐僧</td>
				<td>男</td>
				<td>女儿国</td>
			</tr>
			<tr>
				<td>1</td>
				<td>孙悟空</td>
				<td>男</td>
				<td>花果山</td>
			</tr>
			<tr>
				<td>2</td>
				<td>猪八戒</td>
				<td>男</td>
				<td>高老庄</td>
			</tr>
			<tr>
				<td>3</td>
				<td>沙和尚</td>
				<td>男</td>
				<td>流沙河</td>
			</tr>
			<tr>
				<td>4</td>
				<td>唐僧</td>
				<td>男</td>
				<td>女儿国</td>
			</tr>
		</table>
		
	</body>
</html>

```

<br/>

<br/>

### 长表格

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		
		<table>
			<!-- 
				有一些情况下表格是非常的长的，
					这时就需要将表格分为三个部分，表头，表格的主体，表格底部
				在HTML中为我们提供了三个标签：
					thead 表头
					tbody 表格主体
					tfoot 表格底部
					
				这三个标签的作用，就来区分表格的不同的部分，他们都是table的子标签，
					都需要直接写到table中，tr需要写在这些标签当中
					
				thead中的内容，永远会显示在表格的头部
				tfoot中的内容，永远都会显示表格的底部
				tbody中的内容，永远都会显示表格的中间
				
				如果表格中没有写tbody，浏览器会自动在表格中添加tbody
				并且将所有的tr都放到tbody中，所以注意tr并不是table的子元素，而是tbody的子元素
				通过table > tr 无法选中行 需要通过tbody > tr
			-->
			<thead>
				<tr>
					<th>日期</th>
					<th>收入</th>
					<th>支出</th>
					<th>合计</th>
				</tr>
			</thead>
			
			<tfoot>
				<tr>
					<td></td>
					<td></td>
					<td>合计</td>
					<td>100</td>
				</tr>
			</tfoot>
			
			<tbody>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
				<tr>
					<td>10.24</td>
					<td>500</td>
					<td>300</td>
					<td>200</td>
				</tr>
			</tbody>
			
			
			
		</table>
		
	</body>
</html>

```

<br/>

<br/>

### 表格的布局

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<!-- 
			以前表格更多的情况实际上是用来对页面进行布局的，但是这种方式早已被CSS所淘汰了
			表格的列数由td最多的那行决定
			表格是可以嵌套，可以在td中在放置一个表格
		-->
		
		<table border="1" width="100%">
			<tr height="100px">
				<td colspan="2"></td>
			</tr>
			<tr height="400px">
				<td width="20%"></td>
				<td width="80%">
					<table border="1" width="100%" height="100%">
						<tr>
							<td></td>
						</tr>
						<tr>
							<td></td>
						</tr>
					</table>
				</td>
			</tr>
			<tr height="100px">
				<td colspan="2"></td>
			</tr>
		</table>
		
		
	</body>
</html>

```

<br/>

<br/>

### 完善clearfix

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 300px;
				height: 300px;
				background-color: #bfa;
			}
			.box2{
				width: 200px;
				height: 200px;
				background-color: yellow;
				
				/*
				 * 子元素和父元素相邻的垂直外边距会发生重叠，子元素的外边距会传递给父元素
				 * 使用空的table标签可以隔离父子元素的外边距，阻止外边距的重叠
				 */
				margin-top: 100px;
			}
			
			
			
			
			.box3{
				border: 10px red solid;
			}
			
			.box4{
				width: 100px;
				height: 100px;
				background-color: yellowgreen;
				float: left;
			}
			
			/**
			 * 解决父子元素的外边距重叠
			 */
			/*.box1:before{
				content: "";*/
				/*
				 * display:table可以将一个元素设置为表格显示
				 */
			/*	display: table;
			}
			*/
			
			/**
			 * 解决父元素高度塌陷
			 */
			/*.clearfix:after{
				content: "";
				display: block;
				clear: both;
			}*/
			
			/*
			 * 经过修改后的clearfix是一个多功能的
			 * 	既可以解决高度塌陷，又可以确保父元素和子元素的垂直外边距不会重叠
			 */
			.clearfix:before,
			.clearfix:after{
				content: "";
				display: table;
				clear: both;
			}
			
			.clearfix{
				zoom: 1;
			}
		</style>
	</head>
	<body>
		
		<div class="box3 clearfix">
			<div class="box4"></div>
		</div>
		
		<div class="box1 clearfix">
			<div class="box2"></div>
		</div>
		
	</body>
</html>

```

<br/>

<br/>

### 表单

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<!-- 
			表单的作用就是用来将用户信息提交给服务器的
				比如：百度的搜索框 注册 登录这些操作都需要填写表单
		-->
		
		
		<!--
			使用form标签创建一个表单
				form标签中必须指定一个action属性，该属性指向的是一个服务器的地址
				当我们提交表单时将会提交到action属性对应的地址	
		-->
		<form action="target.html">
			<!-- 
				使用form创建的仅仅是一个空白的表单，
					我们还需要向form中添加不同的表单项
			-->
			
			<!-- 
				在表单中可以使用fieldset来为表单项进行分组，
				可以将表单项中的同一组放到一个fieldset中
			-->
			<fieldset>
				
				<!-- 在fieldset可以使用legend子标签，来指定组名 -->
				<legend>用户信息</legend>
				
				
				<!-- 
					使用input来创建一个文本框，它的type属性是text
						如果希望表单项中的数据会提交到服务器中，还必须给表单项指定一个name属性
						name表示提交内容的名字	
						
					用户填写的信息会附在url地址的后边以查询字符串的形式发送给服务器
						url地址?查询字符串
					格式：
						属性名=属性值&属性名=属性值&属性名=属性值&属性名=属性值
					在文本框中也可以指定value属性值，该值将会作为文本框的默认值显示	
				-->
				<!-- 
					在html中还为我们提供了一个标签，专门用来选中表单中的提示文字的
					label标签
					该标签可以指定一个for属性，该属性的值需要指定一个表单项的id值
				-->
				<label for="um">用户名</label>
				<input id="um" type="text" name="username"  /> <br /><br />
				
				<!--
					密码框
						- 使用input创建一个密码框，它的type属性值是password
				-->
				<label for="pwd">密码 </label>
				<input id="pwd" type="password" name="password" /> <br /><br />
			</fieldset>
			
			<fieldset >
				
				<legend>用户爱好</legend>
			
			<!--
				单选按钮
					- 使用input来创建一个单选按钮，它的type属性使用radio
					- 单选按钮通过name属性进行分组，name属性相同是一组按钮
					- 像这种需要用户选择但是不需要用户直接填写内容的表单项，
						还必须指定一个value属性，这样被选中的表单项的value属性值将会最终提交给服务器
						
					如果希望在单选按钮或者是多选框中指定默认选中的选项，
						则可以在希望选中的项中添加checked="checked"属性
			-->
			性别  <input type="radio" name="gender" value="male" id="male" /><label for="male">男</label>
				<input type="radio" name="gender" value="female" checked="checked" id="female" /><label for="female">女</label> 
				<br /><br />
			
			<!-- 
				多选框
					- 使用input创建一个多选框，它的type属性使用checkbox
			-->
			爱好 	<input type="checkbox" name="hobby" value="zq" />足球
				<input type="checkbox" name="hobby" value="lq" />篮球
				<input type="checkbox" name="hobby" value="ymq" checked="checked" />羽毛球
				<input type="checkbox" name="hobby" value="ppq" checked="checked"/>乒乓球
			<br /><br />
			
			</fieldset>
			
			<!-- 
				下拉列表
					- 使用select来创建一个下拉列表
					下拉列表的name属性需要指定给select，而value属性需要指定给option
					可以通过在option中添加selected="selected"来将选项设置为默认选中
					
					当为select添加一个multiple="multiple"，则下拉列表变为一个多选的下拉列表
			-->
			你喜欢的明星 
				<select name="star">
					
					<!-- 
						在select中可以使用optgroup对选项进行分组
							同一个optgroup中的选项是一组
						可以通过label属性来指定分组的名字	
					-->
					<optgroup label="女明星">
						<!-- 在下拉列表中使用option标签来创建一个一个列表项 -->
						<option value="fbb">范冰冰</option>
						<option value="lxr">林心如</option>
						<option value="zw">赵薇</option>
					</optgroup>
					
					<optgroup label="男明星">
						<option value="zbs" selected="selected">赵本山</option>
						<option value="ldh">刘德华</option>
						<option value="pcj">潘长江</option>
					</optgroup>
					
				</select>
			
			<br /><br />
			
			<!--
				使用textarea创建一个文本域
			-->
			自我介绍  <textarea name="info"></textarea>
			
			<br /><br />
			
			<!-- 
				提交按钮可以将表单中的信息提交给服务器
				使用input创建一个提交按钮,它的type属性值是submit
				在提交按钮中可以通过value属性来指定按钮上的文字
			-->
			<input type="submit" value="注册" />
			
			<!--
				<input type="reset" />可以创建一个重置按钮，
					点击重置按钮以后表单中内容将会恢复为默认值
			-->
			<input type="reset" />
			
			<!-- 
				使用input type=button可以用来创建一个单纯的按钮，
					这个按钮没有任何功能，只能被点击
			-->
			<input type="button" value="按钮" />
			
			<!--
				除了使用input，也可以使用button标签来创建按钮
				这种方式和使用input类似，只不过由于它是成对出现的标签
					使用起来更加的灵活
			-->
			<br /><br />
			<button type="submit">提交</button>
			<button type="reset">重置</button>
			<button type="button">按钮</button>
			
		</form>
	</body>
</html>

```

<br/>

<br/>

### 框架集

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
		<!-- 
			框架集和内联框架的作用类似，都是用于在一个页面中引入其他的外部的页面，
				框架集可以同时引入多个页面，而内联框架只能引入一个，
				在h5标准中，推荐使用框架集，而不使用内联框架
				
			使用frameset来创建一个框架集，注意frameset不能和body出现在同一个页面中
				所以要使用框架集，页面中就不可以使用body标签
			
			属性：
				rows，指定框架集中的所有的框架，一行一行的排列
				cols， 指定框架集中的所有的页面，一列一列的排列
				
				这两个属性frameset必须选择一个，并且需要在属性中指定每一部分所占的大小
				
				frameset中也可以再嵌套frameset
				
			frameset和iframe一样，它里边的内容都不会被搜索引擎所检索，
				所以如果搜索引擎检索到的页面是一个框架页的话，它是不能去判断里边的内容的
			使用框架集则意味着页面中不能有自己的内容，只能引入其他的页面，而我们每单独加载一个页面
				浏览器都需要重新发送一次请求，引入几个页面就需要发送几次请求，用户的体验比较差
			如果非得用建议使用frameset而不使用iframe	
				
		-->
		<frameset cols="30% , * , 30%">
			<!-- 在frameset中使用frame子标签来指定要引入的页面 
				引入几个页面就写几个frame
			-->	
			<frame src="01.表格.html" />
			<frame src="02.表格.html" />
			<!-- 嵌套一个frameset -->
			<frameset rows="30%,50%,*">
				<frame src="04.表格的布局.html" />
				<frame src="05.完善clearfix.html" />
				<frame src="06.表单.html" />
			</frameset>
		</frameset>
</html>

```

<br/>

<br/>

### 图片

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			.box1{
				width: 200px;
				height: 200px;
				background-image: url(img/3.png);
				background-repeat: no-repeat;
			}
			.box2{
				width: 200px;
				height: 200px;
				background-image: url(img/4.png);
				background-repeat: no-repeat;
			}
			
		</style>
	</head>
	<body style="background-color: #bfa;">
		<!-- 
			在IE6中对图片格式png24支持度不高，
				如果使用的图片格式是png24，则会导致透明效果无法正常显示
			解决方法：
				1.可以使用png8来代替png24，即可解决问题，
					但是使用png8代替png24以后，图片的清晰图会有所下降
					
				2.使用JavaScript来解决该问题，需要向页面中引入一个外部的JavaScript文件
					然后在写一下简单的JS代码，来处理该问题
					
		-->
		<div class="box1"></div>
		<div class="box2"></div>
		<img src="img/3.png"/>
		
		<!-- 在body标签的最后引入外部的JS文件 -->
		<script type="text/javascript" src="js/DD_belatedPNG_0.0.8a-min.js"></script>
		<!--再创建一个新的script标签，并且编写一些js代码 -->
		<script type="text/javascript">
			DD_belatedPNG.fix("*");
		</script>
	</body>
</html>

```

<br/>

<br/>

### 条件Hack

```
<!--[if <keywords>? IE <version>?]>
HTML代码块
<![endif]-->

<keywords> 
if条件共包含6种选择方式：是否、大于、大于或等于、小于、小于或等于、非指定版本

是否： 指定是否IE或IE某个版本。关键字：空 
大于： 选择大于指定版本的IE版本。关键字：gt（greater than） 
大于或等于： 选择大于或等于指定版本的IE版本。关键字：gte（greater than or equal） 
小于： 选择小于指定版本的IE版本。关键字：lt（less than） 
小于或等于： 选择小于或等于指定版本的IE版本。关键字：lte（less than or equal） 
非指定版本： 选择除指定版本外的所有IE版本。关键字：!

<version> 
目前的常用IE版本为6.0及以上，推荐酌情忽略低版本，把精力花在为使用高级浏览器的用户提供更好的体验上

IE10及以上版本已将条件注释特性移除，使用时需注意。

```



##### 是否，示例代码：

```
<!--[if IE]>
<style>
.test{color:red;}
</style>
<![endif]-->
```

<br/>

<br/>

### 属性级Hack

```
selector{<hack>?property:value<hack>?;}

_： 选择IE6及以下。连接线（中划线）（-）亦可使用，为了避免与某些带中划线的属性混淆，所以使用下划线（_）更为合适。
*： 选择IE7及以下。诸如：（+）与（#）之类的均可使用，不过业界对（*）的认知度更高 
\9： 选择IE6+ 
\0： 选择IE8+和Opera15以下的浏览器 

选择不同的浏览器及版本 
尽可能减少对CSS Hack的使用。Hack有风险，使用需谨慎 
通常如未作特别说明，本文档所有的代码和示例的默认运行环境都为标准模式。 
一些CSS Hack由于浏览器存在交叉认识，所以需要通过层层覆盖的方式来实现对不同浏览器进行Hack的。如下面这个例子：

```



```
<!DOCTYPE html>
<html lang="zh-cmn-Hans">
<head>
<meta charset="utf-8" />
<title>属性级Hack_CSS参考手册_web前端开发参考手册系列</title>
<meta name="author" content="Joy Du(飘零雾雨), dooyoe@gmail.com, www.doyoe.com" />
<style>
h1 {
	margin: 10px 0;
	font-size: 16px;
}
.test {
	color: #c30;          /* For latest Firefox, chrome, Safari */
	color: #090\0;        /* For Opera15- */
	color: #00f\9;        /* For IE8+ */
	*color: #f00;         /* For IE7 */
	_color: #ff0;         /* For IE6 */
}
</style>
</head>
<body>
<div class="test">在不同浏览器下看看我的颜色吧</div>
</body>
</html>
			
```

<br/>

<br/>

### 选择符级Hack

```
* html .test { color: #090; }       /* For IE6 and earlier */  ie6和ie6之前的浏览器可以见
* + html .test { color: #ff0; }     /* For IE7 */  ie8的浏览器可以见
.test:lang(zh-cmn-Hans) { color: #f00; }  /* For IE8+ and not IE */ ie8及以上和其他的浏览器可以见,
.test:nth-child(1) { color: #0ff; } /* For IE9+ and not IE */ ie9及以上和其他的浏览器可以见,
```

