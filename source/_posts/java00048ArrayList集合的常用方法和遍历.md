---
title: ArrayList集合的常用方法和遍历
date: 2020-02-13 16:08:58
tags:
categories: Java
doc:
---

# ArrayList集合的常用方法和遍历

#### `public boolean add(E)`方法:向集合中添加一个元素

#### `public E remove(int index)`方法:在集合中移除一个元素

#### `public E get(int index)`方法:获取指定的集合元素

#### `public int size()`方法:获取集合的长度

#### 索引从0开始

#### 小技巧:

##### &emsp;输入`对象数组名.fori`再按下tab键,会自动生成一个

```java
for(int i = 0 ;i < 对象名.size();i++)
{

}
```



```java

import java.util.ArrayList;

public class Main14{
    public static void main(String[] args) {
        ArrayList<Dog> dog = new ArrayList<>();
        //向dog集合添加元素
        dog.add(new Dog("小黑",1,"黑色"));
        dog.add(new Dog("小黄",2,"黄色"));
        dog.add(new Dog("小白",3,"白色"));
        dog.add(new Dog("小pi",4,"黑白色"));
        printAll(dog);
        //移除dog集合中的第2号元素,索引从0开始;
        dog.remove(2);
        printAll(dog);

    }
    public static void printAll(ArrayList<Dog> dog)
    {
        //获取dog集合的长度
        for (int i = 0; i < dog.size(); i++) {
            //获取dog集合中指定第i个元素,从索引0开始
            dog.get(i).printAll();
            System.out.println("============================================");
        }
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
    } //打印所有成员变量



}

```

