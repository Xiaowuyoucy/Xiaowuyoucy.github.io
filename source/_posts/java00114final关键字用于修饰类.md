---
title: java final关键字用于修饰类
date: 2020-03-12 15:31:37
tags:
categories: Java
doc:
---

# java final关键字用于修饰类


当final关键字用来修饰一个类的时候，

### 格式：

```java
public final class 类名称 {
    // ...
}
```

### 含义：

当前这个类不能有任何的子类。（太监类）

### 注意：

一个类如果是final的，那么其中所有的成员方法都无法进行覆盖重写（因为没儿子。）


```java
package cn.itcast.day11.demo01;

public final class MyClass /*extends Object*/ {

    public void method() {
        System.out.println("方法执行！");
    }

}

```



```java
package cn.itcast.day11.demo01;

// 不能使用一个final类来作为父类
public class MySubClass /*extends MyClass*/ {
}

```

