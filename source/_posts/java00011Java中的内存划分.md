---
title: Java中的内存划分
date: 2020-02-07 23:10:32
tags:
categories: Java
doc:
---

## Java的内存需要划分成为5个部分

#### 1.<span style = "color:red;">栈（Stack）</span>: 存放的都是方法中的局部变量。方法的运行一定要在栈当中运行。

&emsp;&emsp;&emsp;局部变量：方法的参数，或者是方法{}内部的变量

&emsp;&emsp;&emsp;作用域：一但超出作用域，立刻从栈内存中消失。

#### 2.<span style = "color:red;">堆（Heap）：凡是new出来的内存，都在堆区中。</span>

 &emsp;  &emsp; 堆内存里面的内存都有一个内存地址值：16进制

&emsp; &emsp;  堆内存里面的数据，都有默认值。规则：

 &emsp;  &emsp; 如果是整数 &emsp;  &emsp;  &emsp; &emsp;&nbsp;默认值为0

 &emsp;  &emsp; 如果是浮点数 &emsp;  &emsp;  &emsp;&nbsp;&nbsp;默认值为0.0

 &emsp;  &emsp; 如果是字符&nbsp; &emsp;  &emsp;  &emsp; &emsp;默认值为‘\u0000’

 &emsp;  &emsp; 如果是布尔 &emsp;  &emsp;  &emsp; &nbsp;&emsp;默认值为false

 &emsp;  &emsp; 如果是引用类型&emsp;&emsp;&emsp;&emsp;默认值为null

#### 3.<span style = "color:red;">方法区（Method Area）</span>：存储.class相关的信息，包含方法的信息



#### 4.<span style = "color:red;">本地方法栈（Native Method Stack）</span>：与操作系统相关。

#### 5.<span style = "color:red;">寄存器（PC Register）</span>：与CPU相关。