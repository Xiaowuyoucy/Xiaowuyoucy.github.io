---
title: this关键字
date: 2020-02-12 08:09:38
tags:
categories: Java
doc:
---

#  this关键字

##### 当方法的局部变量和类的成员变量重名时,工具"就近原则",优先使用局部变量.

##### 如果需要访问本类的当中的成员变量,需要使用格式:

##### `this.变量名`

##### 通过谁调用的方法,谁就是this.

```java
public class Main01{
    public static void main(String[] args) {
        Person person = new Person();

        person.name = "王思聪";
        person.printWho("王健林");
    }
}

```

```java
public class Person{
    public String name;
    public void printWho(String name)
    {
        //局部变量name和成员变量name重名,所以用this区分
        System.out.println("你好,我是" + name + "\n你好,我是" + this.name); // \n是换行符

    }
}
```

