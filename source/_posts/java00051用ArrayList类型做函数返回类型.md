---
title: 用ArrayList类型做函数返回类型
date: 2020-02-15 21:44:45
tags:
categories: Java
doc:
---

# 用ArrayList类型做函数返回类型

### 格式:

`public static ArrayList<引用类型> 函数名(类型名 变量名 )`

```java

import java.util.ArrayList;
import java.util.Random;

/*
随机存储20位数到ArrayList中,通过函数调用筛选偶数出来,并存储在一个集合中,然后返回一个只装有偶数的集合.
*/
public class demo05{
    public static void main(String[] args) {
        ArrayList<Integer> intList = new ArrayList<>();
        ArrayList<Integer> smallList;
        Random r = new Random();
        for (int i = 0; i < 20; i++) {
            intList.add(r.nextInt(1000));
        }
        smallList = isOuShu(intList);
        System.out.println("偶数总个数: " + smallList.size());
        System.out.println(smallList);
    }
	//用 ArrayList<Integer>做函数返回类型
    public static ArrayList<Integer> isOuShu(ArrayList<Integer> intList)
    {
        ArrayList<Integer> smallList = new ArrayList<>();
        for (int i = 0; i < intList.size(); i++) {
            int num = intList.get(i);
            if(num % 2 == 0)
            {
                smallList.add(num);
            }
        }
        return smallList;
    }
}

```

