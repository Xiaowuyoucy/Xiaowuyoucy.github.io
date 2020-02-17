---
title: 转换为Boolean
date: 2020-02-17 16:58:33
tags:
categories: JS
doc:
---

# 转换为Boolean

### 将其他类型转换为Boolean

使用`Boolean()函数`

除了0和NaN,其他的都为`true`

字符串转布尔,除了空串,其余的都是`true.`

`null`和`undefined`都会转换为`false`

对象也会转换为`true`

```js
var a = 123;
var b = 0;
var c = NaN;
var d = "";
console.log(Boolean(a));//true
console.log(Boolean(b);//false
console.log(Boolean(c));//false
console.log(Boolean(d));//false
console.log(Boolean(null));//false
console.log(Boolean(undefined));//false

```

<br />

<br />

<br />

<br />

<br />