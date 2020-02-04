---
title: java运算符
date: 2020-02-04 07:09:34
tags:
categories: Java
doc:
---

# 运算符

### 基本运算符

- #### +

- #### -

- #### \*

- #### /

- #### %



### 自增自减

- #### --

- #### ++

### 复合运算符

- #### 　　+=

- ####  　　-=

- #### 　　\*=

- ####  　　/=

- #### 　　 　　%=

  

  默认会强制类型转换

类型不同运算时,返回的结果为较大范围的类型

```java
public class test007 {
    public static void main(String[] args) {

        int a = 10,b = 20,c = 0;

        c = a + b;
        System.out.println(c);

        c = a - b;
        System.out.println(c);
        
        c = a * b;
        System.out.println(c);

        c = a / b;
        System.out.println(c);

        c = a % b;
        System.out.println(c);
        
        c += a / a; // c = c + (a / a)
        System.out.println(c);
    }
}
```



### String 字符串类型

##### 	String 变量名

字符串 + 字符串 = 拼接字符串

```java
public class test008 {
    public static void main(String[] args) {

        String      str = "abc";
        System.out.println(str + "efg");

    }
}

```



### 比较运算符

- #### \>　　　大于

- #### <　　　小于

- #### =　　　等于

- #### <=　　小于等于

- #### >=　　等于等于

- #### ==　　等于

- #### !=　　不等于

运算结果都是布尔值

```java
public class test008 {
    public static void main(String[] args) {
		boolean isTrue;
        int a = 100,b = 99;
        isTrue = a > b;
        System.out.println(isTrue);


    }
}

```



### 逻辑运算符

- ##### &&(与),||(或),!(非)

  - && 	两个条件为真,结果才为真
  - ||  	只要有一个条件为真,结果就为真
  - ! 		真就是假,假就是真

```java
public class test008 {
    public static void main(String[] args) {

		boolean isTrue;
        int a = 100,b = 99,c = 101;
        isTrue = a > b && a < c;
        System.out.println(isTrue);

    }
}

```



### 三元运算符

- ##### 变量名 = 条件判断 ? 表达式A :表达式B

  - ​		条件为真返回表达式A,否则返回表达式B
  - ​		两个表达式类型要和变量类型一致
  - ​		运算结果必须被使用

```java
public class test008 {
    public static void main(String[] args) {

        int a = 100,b = 99,c;
        
        c = (a > b)? a : b;

    }
}

```



