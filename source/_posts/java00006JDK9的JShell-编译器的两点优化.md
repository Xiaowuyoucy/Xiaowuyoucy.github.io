---
title: JDK9的JShell-编译器的两点优化
date: 2020-02-04 08:17:40
tags:
categories: Java
doc:
---

# JDK9的JShell

 什么时候会用到 JShell 工具呢，当我们编写的代码非常少的时候，而又不愿意编写类，main方法，也不愿意去编译和运 行，这个时候可以使用JShell工具。 启动JShell工具，在DOS命令行直接输入JShell命令。 

打开cmd

![](/images/javawz/微信截图_20200204082417.png)

​		

输入jshell

![](/images/javawz/微信截图_20200204082552.png)

​		

退出/exit

![](/images/javawz/微信截图_20200204082647.png)







# 编译器的两点优化

byte,short,char类型右侧没有超过本身范围,会自动强制转换类型
		byte a = 12
		常量优化
		1 + 1常量表达式 在编译时会计算结果,而不是在运行时计算

```java
public class test010 {
    public static void main(String[] args) {
        byte a = 12 //隐式的把12从int类型转换为byte类型
    }
}

```

