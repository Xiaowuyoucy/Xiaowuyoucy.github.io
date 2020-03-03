---
title: java数学工具类Math
date: 2020-03-03 12:10:55
tags:
categories: Java
doc:
---

# java数学工具类Math

`java.lang.Math`类是数学相关的工具类,里面提供了大量的静态方法,完成与数学运算的操作.

`public static double abs(double num)`:获取绝对值.

`public static double ceil(double num)`:向上取整.

`public static double floor(double num)`:向下取整.

`public static long round(double num)`:四舍五入.

`Math.PI`代表圆周率

```java
package xiaochenyan.top.Demo;

public class Main20{
    public static void main(String[] args) {
        double a = 31.2;
        double b = 31.9;
        double c = 31.0;
        //取绝对值
        System.out.println(Math.abs(-31));//32
        System.out.println("==========================================");

        //向上取整
        System.out.println(Math.ceil(a));//32
        System.out.println(Math.ceil(b));//32
        System.out.println(Math.ceil(c));//32
        //向下取整
        System.out.println("==========================================");
        System.out.println(Math.floor(a));//31
        System.out.println(Math.floor(b));//31
        System.out.println(Math.floor(c));//31

        System.out.println("==========================================");
        //四舍五入
        System.out.println(Math.round(a));//31
        System.out.println(Math.round(b));//32
        System.out.println(Math.round(c));//31

        System.out.println("==========================================");
        //圆周率
        System.out.println(Math.PI);//3.141592653589793

    }
}


```

