---
title: ArrayList集合存储基本数据
date: 2020-02-15 21:10:36
tags:
categories: Java
doc:
---

# ArrayList集合存储基本数据

泛型只能是引用类型,不能是基本类型

如果希望向集合ArrayList当中存储基本类型对应的"包装类"。

| 基本类型 | 包装类(引用类型,包装类都位于java.lang包下) |
| :------: | :----------------------------------------: |
|   byte   |                    Byte                    |
|  short   |                   Short                    |
|   int    |                  Integer                   |
|   long   |                    Long                    |
|  float   |                   Float                    |
|  double  |                   Double                   |
|   char   |                 Character                  |
| boolean  |                  Boolean                   |

从JDK 1.5+开始,支持自动装箱和自动拆箱

自动装箱:基本类型自动转换到包装类型

自动拆箱:包装类型自动转换到基本类型

使用格式:

`ArrayList<Integer> intVal = new ArrayList<>();`

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class demo01{
    public static void main(String[] args) {
        //使用包装类
        ArrayList<Integer> intList = new ArrayList<>();
        Scanner sc = new Scanner(System.in);
        //向列表加入元素
        intList.add(888);
        intList.add(89);
        intList.add(sc.nextInt());
        //输出列表
        System.out.println(intList);
        System.out.println(intList.get(1));

    }
}

```

