---
title: Map常用子类
date: 2020-04-04 23:17:33
tags:
categories: Java
doc:
---

#   Map常用子类

通过查看Map接口描述，看到Map有多个子类，这里我们主要讲解常用的HashMap集合、LinkedHashMap集合。

* **HashMap<K,V>**：存储数据采用的哈希表结构，元素的存取顺序不能保证一致。由于要保证键的唯一、不重复，需要重写键的hashCode()方法、equals()方法。
* **LinkedHashMap<K,V>**：HashMap下有个子类LinkedHashMap，存储数据采用的哈希表结构+链表结构。通过链表结构可以保证元素的存取顺序一致；通过哈希表结构可以保证的键的唯一、不重复，需要重写键的hashCode()方法、equals()方法。

> tips：Map接口中的集合都有两个泛型变量<K,V>,在使用时，要为两个泛型变量赋予数据类型。两个泛型变量<K,V>的数据类型可以相同，也可以不同。

` java.util.HashMap<k,v>`集合 `implements Map<k,v>`接口

​    `HashMap`集合的特点:

​        1.`HashMap`集合底层是哈希表:查询的速度特别的快

​            `JDK1.8`之前:数组+单向链表

​            `JDK1.8`之后:`数组+单向链表|红黑树(链表的长度超过8)`:提高查询的速度

​        2.`hashMap`集合是一个无序的集合,存储元素和取出元素的顺序有可能不一致

  ` java.util.LinkedHashMap<k,v>`集合` extends HashMap<k,v>`集合

  ` LinkedHashMap`的特点:

​        1.LinkedHashMap集合底层是`哈希表+链表(保证迭代的顺序)`

​        2.LinkedHashMap集合是一个`有序`的集合,存储元素和取出元素的`顺序是一致`的