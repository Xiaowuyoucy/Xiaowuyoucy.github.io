---
title: 元素的伪类选择器
date: 2020-05-30 19:43:22
tags:
categories: HTML/CSS
doc:
---

# 元素的伪类选择器

伪类专门用来表示元素的一种特殊的状态.

比如访问过的超链接等

![1590839109726](/images/javawz/1590839109726.png)

`:link`表示普通的链接（没访问过的超链接）

```css
a:link{
color:yellow;
}
```

`:visited`表示访问过的超链接

浏览器是通过历史记录来判断一个链接是否访问过.

 由于隐私的原因，所以visited这个伪类只能修改链接的颜色

```css
a:visited{
color:red;
}
```

`:hover`表示鼠标移入的状态

```css
a:hover{
color:red;
}
```

`:active`表示的是超链接被点击的状态.

```css
a:active{
color:red;
}
```

`:hover`和`:active`也可以为其他元素设置

```css
p:hover{
color:red;
}
p:active{
color:red;
}
```

IE6中,不支持对超链接以为的元素设置`:hover`和`:active`



`:focus`获取焦点

```css
input:focus{
background-color:yellow;
}
```

`::selection`为p标签中选中的内容使用样式

可以用::selection伪类

注意:这个伪类在火狐浏览器中需要采用另外一种方式编写`::-moz-selection`

```css
p::-moz-selection{

}
p::selection{

}
```

