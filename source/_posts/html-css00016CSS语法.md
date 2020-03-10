---
title: CSS语法
date: 2020-03-05 15:11:34
tags:
categories: HTML/CSS
doc:
---

# CSS语法

### CSS中的注释，注释中的内容会自动被浏览器所忽略

HTML中的`<!---->`注释在`<style>`标签内或css文件中是不能使用的

##### css注释语法

```css
/*
注释内容
*/
```



### CSS的基本语法:

##### 选择器 声明块

```css
p{
    属性名1:值;
    属性名2:值;
}
```

选择器，通过选择器可以选中页面中的指定元素
比如 p 的作用就是选中页面中所有的p元素

声明块，通过声明块来指定要为元素设置的样式
声明块由一个一个的声明组成
声明是一个名值对结构
一个样式名对应一个样式值，名和值之间以`:`连接，以`;`结尾   

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>

      p{
          color: red;
          font-size: 40px;
      }

      h1{
          color: green;
      }

      
    
    </style>
</head>
<body>
    <h1>我是H1</h1>
    <p>今天天气真不错！</p>
    <p>今天天气真不错！</p>
    <p>今天天气真不错！</p>
    <p>今天天气真不错！</p>
</body>
</html>
```

