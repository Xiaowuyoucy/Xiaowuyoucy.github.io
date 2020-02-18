---
title: 静态static关键字修饰成员变量
date: 2020-02-19 06:58:40
tags:
categories: Java
doc:
---

# 静态static关键字修饰成员变量

如果一个成员变量使用了`static`关键字,那么这个变量就不再属于对象自己,而是属于所在的类.多个对象共享同一份数据.

<br>

<br>

#### 格式:

```java
static int number;
public static String name;
private static float jiGe;
```

<br>

<br>

#### 使用静态成员变量:

`类名.静态成员变量;`

`对象名.静态成员变量;`

<br>

<br>

修改静态成员变量的值:

`类名.静态成员变量 = 1;`

`对象名.静态成员变量 = 1;`

<br>

```java


public class Main15{
    public static void main(String[] args) {
        Student one = new Student("张三丰",102);
        Student two = new Student("张无极",23);
        Student three = new Student("赵敏",18);
        Student four = new Student("周芷若",18);
        Student.room = "101教师";
        System.out.println("名字:" + one.getName() + "  年龄:"
                + one.getAge() + "  课室" + one.room
                + "  学号:" + one.getId() );

        System.out.println("名字:" + two.getName() + "  年龄:"
                + two.getAge() + "  课室" + two.room
                + "  学号:" + two.getId() );


        System.out.println("名字:" + three.getName() + "  年龄:"
                + three.getAge() + "  课室" + three.room
                + "  学号:" + three.getId() );

        System.out.println("名字:" + four.getName() + "  年龄:"
                + four.getAge() + "  课室" + four.room
                + "  学号:" + four.getId() );
    }
}

```

```java

public class Student{
    private String name;
    private int    age;
    public static String  room;
    private int id;
    private static int countIdNumber;

    public int getId() {
        return id;
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

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        id = ++countIdNumber;
    }

    public Student() {
        id = ++countIdNumber;
    }
}

```

