---
title: 静态static关键字修饰成员方法
date: 2020-02-19 07:57:16
tags:
categories: Java
doc:
---

# 静态static关键字修饰成员方法

1.一旦使用`static`修饰成员方法,那么这就成为了静态方法.

2.静态方法不属于对象,而是属于类的.

3.如果没有用`static`关键字,那么首先必须先创建对象,然后通过对象才能使用成员方法.

4.对于静态方法来说,可以通过对象名进行调用,也可以通过类名来调用.但推荐使用类名来调用.

5.如果是使用对象名来调用静态成员变量,在编译之后也会被`javac`翻译成为`类名.静态方法名`

6.如果有了`static`关键字,那么不需要创建对象,直接就能通过类名称来使用它

7.无论是成员变量还是成员方法,如果有了`static`,都推荐使用`类名称`进行调用.

8.静态成员变量:`类名称.静态变量;`

9.静态方法:`类名称.静态方法();`

10.对于在本类当中的静态方法,可以省略类名称.

<br>

<br>

##### 注意:

1.静态不能直接访问非静态.

原因:因为在内存当中,是先有的静态内容,后有的非静态内容.

<br>

2.静态方法不能使用this.

原因:this代表当前对象,通过谁调用的方法,谁就是对象.



```java

public class Main16{
    public static void main(String[] args) {
        Student2 stu = new Student2("小明");
        Student2.room = "101课室";


        //使用成员方法
        stu.showVar();

        //使用静态成员方法
        Student2.showStaticVar();
        stu.showStaticVar();//不推荐使用对象名调用静态成员方法
    }
}

```

```java
//创建一个Student2类
public class Student2{
    String name;
/*====================================================================*/
    
    public static String room;

    public Student2() {
    }

    public Student2(String name) {
        this.name = name;
    }
    
/*====================================================================*/
    //非静态成员方法
    public void showVar()
    {
        System.out.println("名字: " + this.name);
        System.out.println("年龄: " + Student2.room);

        showStaticVar(); //在本类中可以直接使用静态成员方法,不用加类名
        Student2.showStaticVar(); //加类名使用静态成员方法
    }
    
    //静态成员方法
    //属于类,不属于对象
    public static void showStaticVar(){
        System.out.println("年龄: " + Student2.room);
    }
}

```

