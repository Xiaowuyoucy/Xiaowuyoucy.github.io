---
title: super关键字的三种用法
date: 2020-03-09 17:10:33
tags:
categories: Java
doc:
---
# super关键字的三种用法

### super关键字的用法有三种：

在子类的成员方法中，访问父类的成员变量。

在子类的成员方法中，访问父类的成员方法。

在子类的构造方法中，访问父类的构造方法。

```java
package cn.itcast.day09.demo08;

public class Fu {

    int num = 10;

    public void method() {
        System.out.println("父类方法");
    }

}

```

```java
package cn.itcast.day09.demo08;


public class Zi extends Fu {

    int num = 20;

    public Zi() {
        super();
    }

    public void methodZi() {
        System.out.println(super.num); // 父类中的num
    }

    public void method() {
        super.method(); // 访问父类中的method
        System.out.println("子类方法");
    }

}

```

