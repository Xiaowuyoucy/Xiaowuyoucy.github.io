---
title: java_LinkedList集合
date: 2020-03-22 00:28:37
tags:
categories: Java
doc:
---



`java.util.LinkedList`集合数据存储的结构是链表结构。方便元素添加、删除的集合。

`LinkedList`是一个双向链表，那么双向链表是什么样子的呢，我们用个图了解下

![](/images/javawz/双向链表.png)

实际开发中对一个集合元素的添加与删除经常涉及到首尾操作，而LinkedList提供了大量首尾操作的方法。这些方法我们作为了解即可：

&emsp;&emsp;`public void addFirst(E e)`:将指定元素插入此列表的开头。

&emsp;&emsp;`public void addLast(E e)`:将指定元素添加到此列表的结尾。

&emsp;&emsp;`public E getFirst()`:返回此列表的第一个元素。

&emsp;&emsp;`public E getLast()`:返回此列表的最后一个元素。

&emsp;&emsp;`public E removeFirst()`:移除并返回此列表的第一个元素。

&emsp;&emsp;`public E removeLast()`:移除并返回此列表的最后一个元素。

&emsp;&emsp;`public E pop()`:从此列表所表示的堆栈处弹出一个元素。

&emsp;&emsp;`public void push(E e)`:将元素推入此列表所表示的堆栈。

&emsp;&emsp;`public boolean isEmpty()`：如果列表不包含元素，则返回true。

&emsp;&emsp;`public E remove(int index) ` 移除此列表中指定位置处的元素。将任何后续元素向左移（从索引中减 1）。返回从列表中删除的元素。 

<hr />
LinkedList是List的子类，List中的方法LinkedList都是可以使用。在开发时，LinkedList集合也可以作为堆栈，队列的结构使用。（了解即可）

```java
package com.itheima.demo01.List;

import java.util.LinkedList;

/*
    java.util.LinkedList集合 implements List接口
    LinkedList集合的特点:
        1.底层是一个链表结构:查询慢,增删快
        2.里边包含了大量操作首尾元素的方法
        注意:使用LinkedList集合特有的方法,不能使用多态

        - public void addFirst(E e):将指定元素插入此列表的开头。
        - public void addLast(E e):将指定元素添加到此列表的结尾。
        - public void push(E e):将元素推入此列表所表示的堆栈。

        - public E getFirst():返回此列表的第一个元素。
        - public E getLast():返回此列表的最后一个元素。

        - public E removeFirst():移除并返回此列表的第一个元素。
        - public E removeLast():移除并返回此列表的最后一个元素。
        - public E pop():从此列表所表示的堆栈处弹出一个元素。

        - public boolean isEmpty()：如果列表不包含元素，则返回true。

 */
public class Demo02LinkedList {
    public static void main(String[] args) {
        show03();
    }

    /*
        - public E removeFirst():移除并返回此列表的第一个元素。
        - public E removeLast():移除并返回此列表的最后一个元素。
        - public E pop():从此列表所表示的堆栈处弹出一个元素。此方法相当于 removeFirst
     */
    private static void show03() {
        //创建LinkedList集合对象
        LinkedList<String> linked = new LinkedList<>();
        //使用add方法往集合中添加元素
        linked.add("a");
        linked.add("b");
        linked.add("c");
        System.out.println(linked);//[a, b, c]

        //String first = linked.removeFirst();
        String first = linked.pop();
        System.out.println("被移除的第一个元素:"+first);
        String last = linked.removeLast();
        System.out.println("被移除的最后一个元素:"+last);
        System.out.println(linked);//[b]
    }

    /*
        - public E getFirst():返回此列表的第一个元素。
        - public E getLast():返回此列表的最后一个元素。
     */
    private static void show02() {
        //创建LinkedList集合对象
        LinkedList<String> linked = new LinkedList<>();
        //使用add方法往集合中添加元素
        linked.add("a");
        linked.add("b");
        linked.add("c");

        //linked.clear();//清空集合中的元素 在获取集合中的元素会抛出NoSuchElementException

        //public boolean isEmpty()：如果列表不包含元素，则返回true。
        if(!linked.isEmpty()){
            String first = linked.getFirst();
            System.out.println(first);//a
            String last = linked.getLast();
            System.out.println(last);//c
        }
    }

    /*
        - public void addFirst(E e):将指定元素插入此列表的开头。
        - public void addLast(E e):将指定元素添加到此列表的结尾。
        - public void push(E e):将元素推入此列表所表示的堆栈。此方法等效于 addFirst(E)。
     */
    private static void show01() {
        //创建LinkedList集合对象
        LinkedList<String> linked = new LinkedList<>();
        //使用add方法往集合中添加元素
        linked.add("a");
        linked.add("b");
        linked.add("c");
        System.out.println(linked);//[a, b, c]

        //public void addFirst(E e):将指定元素插入此列表的开头。
        //linked.addFirst("www");
        linked.push("www");
        System.out.println(linked);//[www, a, b, c]

        //public void addLast(E e):将指定元素添加到此列表的结尾。此方法等效于 add()
        linked.addLast("com");
        System.out.println(linked);//[www, a, b, c, com]
    }
}

```

