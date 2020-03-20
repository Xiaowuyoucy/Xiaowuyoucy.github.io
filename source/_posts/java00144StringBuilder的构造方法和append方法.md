---
title: javaStringBuilder的构造方法和append方法
date: 2020-03-15 08:30:13
tags:
categories: Java
doc:
---

# javaStringBuilder的构造方法和append方法


    java.lang.StringBuilder类:字符串缓冲区,可以提高字符串的效率

###     构造方法:

​        StringBuilder() 构造一个不带任何字符的字符串生成器，其初始容量为 16 个字符。
​        StringBuilder(String str) 构造一个字符串生成器，并初始化为指定的字符串内容。




###     StringBuilder的常用方法:

​        public StringBuilder append(...)：添加任意类型数据的字符串形式，并返回当前对象自身。
 

```java
package com.itheima.demo06StringBuilder;

public class Demo01StringBuilder {
    public static void main(String[] args) {
        //空参数构造方法
        StringBuilder bu1 = new StringBuilder();
        System.out.println("bu1:"+bu1);//bu1:""

        //带字符串的构造方法
        StringBuilder bu2 = new StringBuilder("abc");
        System.out.println("bu2:"+bu2);//bu2:abc
    }
}

```

```java
package com.itheima.demo06StringBuilder;

public class Demo02StringBuilder {
    public static void main(String[] args) {
        //创建StringBuilder对象
        StringBuilder bu = new StringBuilder();
        //使用append方法往StringBuilder中添加数据
        //append方法返回的是this,调用方法的对象bu,this==bu
        //StringBuilder bu2 = bu.append("abc");//把bu的地址赋值给了bu2
        //System.out.println(bu);//"abc"
        //System.out.println(bu2);//"abc"
        //System.out.println(bu==bu2);//比较的是地址 true

        //使用append方法无需接收返回值
//        bu.append("abc");
//        bu.append(1);
//        bu.append(true);
//        bu.append(8.8);
//        bu.append('中');
//        System.out.println(bu);//abc1true8.8中

        /*
            链式编程:方法返回值是一个对象,可以继续调用方法
         */
        System.out.println("abc".toUpperCase().toLowerCase().toUpperCase().toLowerCase());
        bu.append("abc").append(1).append(true).append(8.8).append('中');
        System.out.println(bu);//abc1true8.8中

    }
}

```

