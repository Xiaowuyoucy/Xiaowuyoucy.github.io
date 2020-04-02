---
title: java_Set集合存储元素不重复的原理
date: 2020-03-22 01:28:15
tags:
categories: Java
doc:
---



```java
package com.itheima.demo02.Set;

import java.util.HashSet;

/*
    Set集合不允许存储重复元素的原理
 */
public class Demo02HashSetSaveString {
    public static void main(String[] args) {
        //创建HashSet集合对象
        HashSet<String> set = new HashSet<>();
        String s1 = new String("abc");
        String s2 = new String("abc");
        set.add(s1);
        set.add(s2);
        set.add("重地");
        set.add("通话");
        set.add("abc");
        System.out.println(set);//[重地, 通话, abc]
    }

}

```

<br/><br/>

![1584811719822](/images/javawz/1584811719822.png)