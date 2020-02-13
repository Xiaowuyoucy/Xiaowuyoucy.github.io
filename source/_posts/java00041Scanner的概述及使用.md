---
title: Scanner的概述及使用
date: 2020-02-12 19:46:36
tags:
categories: Java
doc:
---

`Scanner`类的功能,可以实现键盘输入数据,到程序当中.

包路径:`import java.util.Scanner;`

#### 使用步骤:

##### 1 . 导包

&emsp;&emsp;`import   包路径.类名称;`

&emsp;&emsp;如果需要使用的目标类,和当前类位于同一个包下,则可以省略导包语句不写.

&emsp;&emsp;在`java.lang`包下的内容不需要导包,其他的包都需要import语句;

&emsp;&emsp;在集成环境中输入`Scanner`按下`回车`会自动进行`导包`;

##### 2.创建

&emsp;&emsp;`类名称 对象名 = new 类名称();`

##### 3.使用

&emsp;&emsp;`对象名.成员方法名()`

获取键盘输入的一个`int`数字,`int num = sc.nextInt();`

获取键盘输入的一串字符串: `String str = sc.next();`



```java
//导包
import java.util.Scanner;

public class Main04{
    public static void main(String[] args) {
        Dog dog = new Dog();
        //创建一个Scanner类
        Scanner sc = new Scanner(System.in);

        System.out.println("请输入名字: ");
        //输入一个字符串
        dog.setName(sc.next());

        System.out.println("请输入年龄: ");
        //输入一个整数
        dog.setAge(sc.nextInt());

        System.out.println("请输入毛色: ");
        //输入一个字符串
        dog.setColor(sc.next());
        System.out.println("==============================");
        dog.printAll();


    }

}

```

```java

public class Dog{
    private String name;//姓名
    private int age;//年龄
    private String color;//毛色

    public Dog(String name, int age, String color) {
        this.name = name;
        this.age = age;
        this.color = color;
    }

    public Dog() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }
    public void printAll() {
        System.out.println("名字: " + name);
        System.out.println("年龄 " + age);
        System.out.println("毛色: " + color);
    }



}

```

