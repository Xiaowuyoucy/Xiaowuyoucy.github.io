---
title: java_HashSet存储自定义类型元素
date: 2020-03-22 01:33:26
tags:
categories: Java
doc:
---

# HashSet存储自定义类型元素



```java
package com.itheima.demo02.Set;

import java.util.HashSet;

/*
    HashSet存储自定义类型元素

    set集合报错元素唯一:
        存储的元素(String,Integer,...Student,Person...),必须重写hashCode方法和equals方法

    要求:
        同名同年龄的人,视为同一个人,只能存储一次
 */
public class Demo03HashSetSavePerson {
    public static void main(String[] args) {
        //创建HashSet集合存储Person
        HashSet<Person> set = new HashSet<>();
        Person p1 = new Person("小美女",18);
        Person p2 = new Person("小美女",18);
        Person p3 = new Person("小美女",19);
        System.out.println(p1.hashCode());//1967205423
        System.out.println(p2.hashCode());//42121758

        System.out.println(p1==p2);//false
        System.out.println(p1.equals(p2));//false
        set.add(p1);
        set.add(p2);
        set.add(p3);
        System.out.println(set);
    }

}

```



```java
package com.itheima.demo02.Set;

import java.util.Objects;

public class Person {
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age &&
                Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {

        return Objects.hash(name, age);
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
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

