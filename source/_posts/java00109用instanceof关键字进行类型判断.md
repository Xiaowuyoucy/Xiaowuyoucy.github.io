---
title: java用instanceof关键字进行类型判断
date: 2020-03-11 17:15:30
tags:
categories: Java
doc:
---

# java用instanceof关键字进行类型判断


如何才能知道一个父类引用的对象，本来是什么子类？

### 格式：

`对象 instanceof 类名称`
这将会得到一个boolean值结果，也就是判断前面的对象能不能当做后面类型的实例。

```java
package cn.itcast.day10.demo06;

public abstract class Animal {

    public abstract void eat();

}

```

```java
package cn.itcast.day10.demo06;

public class Cat extends Animal {
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }

    // 子类特有方法
    public void catchMouse() {
        System.out.println("猫抓老鼠");
    }
}

```

```java
package cn.itcast.day10.demo06;

public class Dog extends Animal {
    @Override
    public void eat() {
        System.out.println("狗吃SHIT");
    }

    public void watchHouse() {
        System.out.println("狗看家");
    }
}


```

```java
package cn.itcast.day10.demo06;

public class Demo02Instanceof {

    public static void main(String[] args) {
        Animal animal = new Dog(); // 本来是一只狗
        animal.eat(); // 狗吃SHIT

        // 如果希望掉用子类特有方法，需要向下转型
        // 判断一下父类引用animal本来是不是Dog
        if (animal instanceof Dog) {
            Dog dog = (Dog) animal;
            dog.watchHouse();
        }
        // 判断一下animal本来是不是Cat
        if (animal instanceof Cat) {
            Cat cat = (Cat) animal;
            cat.catchMouse();
        }

        giveMeAPet(new Dog());
    }

    public static void giveMeAPet(Animal animal) {
        if (animal instanceof Dog) {
            Dog dog = (Dog) animal;
            dog.watchHouse();
        }
        if (animal instanceof Cat) {
            Cat cat = (Cat) animal;
            cat.catchMouse();
        }
    }

}

```

