---
title: ArrayList集合概述和基本使用
date: 2020-02-13 15:54:03
tags:
categories: Java
doc:
---

# ArrayList集合概述和基本使用

#### ` ArrayList`集合的长度是可以变化的

#### 在`java.util`包中

#### `ArrayList`有一个尖括号`<E>`,代表泛型;

#### 泛型也就是代表集合中每一个元素中的类型统一化 ,其中的`E`要`替换`为引用类型

#### 注意:

#### &emsp;泛型只能是引用类型,不能是基本类型.

#### &emsp;在`ArrayList`类中直接打印对象名字显示的不是地址,而是内容

```java
import java.util.ArrayList;

public class Main13{
    public static void main(String[] args) {
        // ArrayList<String>代表strList集合中的元素全部都是String类型
        //new ArrayList<String>() 等价于 new ArrayList<>()
        //从java1.7开始,右侧的尖括号中的内容可以不写,但是<>本身不能省略
        ArrayList<String> strList = new ArrayList<>();

        System.out.println(strList);//[]
        strList.add("张无忌");
        strList.add("张三丰");
        //直接打印对象名
        System.out.println(strList);//["张无忌","张三丰"]
    }
}

```

