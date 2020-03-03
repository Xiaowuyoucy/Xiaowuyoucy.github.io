---
title: java数组工具类Arrays
date: 2020-03-03 11:44:05
tags:
categories: Java
doc:
---

# java数组工具类Arrays

`java.util.Arrays`是一个与数组相关的工具类,里面提供了大量静态方法,用来实现数组常见的操作.

`public static String toString(数组):`

将参数数组变成字符串(按照默认格式: [元素1,元素2,元素3,......])

`public static void sort(数组)`,按照默认升序(从小到大)对数组元素进行排序.

### 备注:

1.如果是数组,sort默认按照升序从小到大

2.如果是字符串,sort默认按照字母升序

3.如果是自定义的类型,那么这个自定义的类需要有Comparable或者Comparator接口支持.

```JAVA
package xiaochenyan.top.Demo;

import java.util.Arrays;
public class Main18{
    public static void main(String[] args) {
        String[] str = {"aaa","ccc","bbb"};
        int[] num = {1,3,5,2,6,34,24};
        //str数组排序
        Arrays.sort(str);
        //将str数组转换为字符串
        System.out.println(Arrays.toString(str));
        //int数组排序
        Arrays.sort(num);
        //将int数组转换为字符串
        System.out.println(Arrays.toString(num));
    }
}
```

