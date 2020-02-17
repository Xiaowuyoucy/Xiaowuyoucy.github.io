---
title: meta标签
date: 2020-02-18 05:09:02
tags:
categories: HTML/CSS
doc:
---

# meta标签

### 使用meta标签还可以用来设置网页的关键字

##### 格式:`<meta name="keywords" content = "关键字" />`

如果是多个关键字,就用逗号隔开.

关键字是被搜索引擎识别的.

```html
<meta name="keywords" content="HTML5,JavaScript,前端,Java" />
```

### 使用meta标签用来指定网页的描述

搜索引擎在检索页面时,会同时检索页面中的关键字和描述,但是这两个值不会影响页面在搜索引擎中的排名.

### 格式:`<meta name="description" content="描述信息" />`

```html
<meta name="description" content="发布h5,js前端等相关信息" />
```



### 使用meta可以用来做请求重定向(页面跳转).

##### 格式:`<meta http-equiv="refresh" content="秒数;url=目标路径" />`

目标路径可以是相对地址

```html
<meta http-equiv="refresh" content="5;http://www.baidu.com"  />
```

