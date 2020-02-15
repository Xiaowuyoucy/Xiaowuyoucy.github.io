---
title: 用ArrayList类型做函数参数
date: 2020-02-15 21:29:09
tags:
categories: Java
doc:
---

# 用ArrayList类型做函数参数



### 格式:

`public static 返回值类型 函数名(ArrayList<引用类型> 变量名 )`



```java

import java.util.ArrayList;

public class demo04{
    public static void main(String[] args) {
        ArrayList<Student> std = new ArrayList<>();
        std.add(new Student("张三丰",99));
        std.add(new Student("张无忌",19));
        std.add(new Student("宋远桥",39));
        std.add(new Student("张翠山",44));
        printArrayList(std);

    }
    //用ArrayList<Student>做函数参数,传递过来的是一个引用的地址
    public static void printArrayList(ArrayList<Student> std){
        System.out.print("{");
        for (int i = 0; i < std.size(); i++) {

            if(i == std.size() - 1)
            {
                System.out.print(std.get(i).getName() + "}");

            }else{

                System.out.print(std.get(i).getName() + "@");
            }
        }
    }
}

```

```java

public class Student{
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Student() {
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
}

```

