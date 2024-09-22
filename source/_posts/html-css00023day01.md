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

