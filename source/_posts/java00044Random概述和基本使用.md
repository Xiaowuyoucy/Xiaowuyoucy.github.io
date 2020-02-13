---
title: Random概述和基本使用
date: 2020-02-13 09:36:07
tags:
categories: Java
doc:
---

# Random概述和基本使用

`Random`类是用来生成随机数字的.使用起来也是三个步骤:

#### 1.导包

`import java.util.Random`

#### 2.创建

`Random rand = new Random();`一般括号留空即可

#### 3.使用

获取一个随机`int`数字(范围是`int`所有范围,有正负两种)

`int num = rand.nextInt();`

获取一个`指定范围`的随机`int`数字(获取范围: [ 0 ,` int `  )   闭合区间 ) 打印范围是`0 ~ (int -1)`

`int num = rand.nextInt(int);`   

```java
import java.util.Random;

public class Main09{
    public static void main(String[] args) {
        Random rand = new Random();
        //范围是负的int最大值~正的int最大值
        System.out.println("随机数是: " + rand.nextInt());
        for(int i = 0; i < 100; i++)
        {
            System.out.println(rand.nextInt(100)); // 打印范围是0~99
        }

    }
}

```

