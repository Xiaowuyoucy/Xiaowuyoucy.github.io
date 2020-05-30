---
title: 常用选择器
date: 2020-03-07 22:29:08
tags:
categories: HTML/CSS
doc:
---

# 常用选择器

### 元素选择器

作用：根据标签名来选中指定的元素
语法：`标签名{}`
例子：p{}  h1{}  div{}

```css
 p{
color: red;
}

h1{
color: green;
} 
```

### id选择器

作用：根据元素的id属性值选中一个元素
语法：`#id属性值{}`

调用:`id="属性值"`

例子：#box{} #red{}  

```css
#red{
color: red;
} 
```

### 类选择器

作用：根据元素的class属性值选中一组元素

id是唯一性的,所以只能应用到一个元素上

语法：`.class属性值`

调用: `class="属性值1 [属性值2 属性值n]"`

```css
 .blue{
color: blue;
}

.abc{
font-size: 20px;
}
```

### 复合选择器(并集选择器)

语法: `选择器1,选择器2,选择器n{}`

```css
.class,h1,#a{
background-color: red;
}
```



### 复合选择器(交集选择器)

作用:可以选中同时满足多个选择器的元素

语法:

`选择器1选择器2选择器N{}`

```html
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title>

		</title>
        <style type="text/css">
            span.p3{
                background-color:red;
            }
        </style>
	</head>

	<body>
        <span class="p3">I love you!</span>
	</body>

</html>
```



### 通配选择器

作用：选中页面中的所有元素
语法: `*{}`

```css
*{
color: red;
}
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        /* 
            将所有的段落设置为红色（字体）

            元素选择器
                作用：根据标签名来选中指定的元素
                语法：标签名{}
                例子：p{}  h1{}  div{}
        */
        /* p{
            color: red;
        }

        h1{
            color: green;
        } */

        /* 
            将儿童相见不相识设置红色


            id选择器
                作用：根据元素的id属性值选中一个元素
                语法：#id属性值{}
                例子：#box{} #red{}  
        */
        /* #red{
            color: red;
        } */

        /*
            将 秋水... 和 落霞... 设置为蓝色
                
            类选择器
                作用：根据元素的class属性值选中一组元素
                语法：.class属性值
        */
        /* .blue{
            color: blue;
        }

        .abc{
            font-size: 20px;
        }
        */

        /* 
            通配选择器
                作用：选中页面中的所有元素
                语法: *
        */
        *{
            color: red;
        }

    
    </style>
</head>
<body>
    <h1 class="blue abc">我是标题</h1>
    <p>少小离家老大回</p>
    <p>乡音无改鬓毛衰</p>
    <p id="red">儿童相见不相识</p>
    <p>笑问客从何处来</p>
    <!-- 
        class 是一个标签的属性，它和id类似，不同的是class可以重复使用
            可以通过class属性来为元素分组
            可以同时为一个元素指定多个class属性
    -->
    <p class="blue">秋水共长天一色</p>
    <p class="blue">落霞与孤鹜齐飞</p>


</body>
</html>
```

