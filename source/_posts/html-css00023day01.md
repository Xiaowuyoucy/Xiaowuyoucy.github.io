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

