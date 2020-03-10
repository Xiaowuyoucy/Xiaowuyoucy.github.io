---
title: super与this关键字的图解
date: 2020-03-09 17:11:29
tags:
categories: Java
doc:
---

# super与this关键字的图解

![](/images/javawz/微信截图_20200309170039.png)

```java
package cn.itcast.day09.demo10;

public class Demo {

    public static void main(String[] args) {
        Zi zi = new Zi();

        zi.show();
        zi.method();
    }

}

```

```java
package cn.itcast.day09.demo10;

public class Fu {

    int num = 10;

    public void method() {
        System.out.println("父类方法");
    }

}

```

```java
package cn.itcast.day09.demo10;

public class Zi extends Fu {

    int num = 20;

    @Override
    public void method() {
        super.method(); // 调用了父类方法
        System.out.println("子类方法");
    }

    public void show() {
        int num = 30;
        System.out.println(num); // 30
        System.out.println(this.num); // 20
        System.out.println(super.num); // 10
    }

}

```

