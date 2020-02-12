---
title: 定义一个标准的类和自动生成构造函数与setter方法/getter方法
date: 2020-02-12 08:33:34
tags:
categories: Java
doc:
---

# 定义一个标准的类和自动生成构造函数与setter方法/getter方法

### 一个标准的类

#### 通常有四个部分组成:

1. ##### 所有的成员变量都要使用`private`关键字修饰

2. ##### 为每一个成员变量编写一对`Getter/Setter`方法

3. ##### 编写一个无参数的构造方法

4. ##### 编写一个全参数的构造方法

   #### 这样标准的类也叫做`Java Bean`

   ### 自动生成构造函数与setter方法/getter方法

   ##### 通常定义一个标准的类只需要写私有成员变量就行了,其余部分交由编译器去生成.

   ##### 生成所有私有变量的setter方法/getter

   `快捷键 : alt + insert`

   #### 生成所有setter方法/getter方法

   ![](/images/javawz/微信截图_20200212080446.png)

   ![](/images/javawz/微信截图_20200212080504.png)

   

   ![](/images/javawz/微信截图_20200212080559.png)

   ### 生成空构造函数

   ![](/images/javawz/微信截图_20200212080446-1581468291840.png)

   

   ![](/images/javawz/微信截图_20200212080638.png)

   

   ![](/images/javawz/微信截图_20200212080657.png)

   ### 生成全参造函数

   ![](/images/javawz/微信截图_20200212080446-1581468387018.png)

   ![](/images/javawz/微信截图_20200212080638-1581468409682.png)

   

   ![](/images/javawz/微信截图_20200212080724.png)

   

   ```java
   
   public class Main03{
       public static void main(String[] args) {
           //使用默认构造函数
           Dog dog1 = new Dog();
           dog1.setName("bobo");
           dog1.setAge(2);
           dog1.setColor("white");
           System.out.println("姓名: " + dog1.getName() + "  年龄: " + dog1.getAge() + "  毛色: " + dog1.getColor());
           System.out.println("====================");
           //使用全参构造函数
           Dog dog2 = new Dog("copi",1,"blackAndYellow");
           //使用getter方法
           System.out.println("姓名: " + dog2.getName() + "  年龄: " + dog2.getAge() + "  毛色: " + dog2.getColor());
           //使用setter方法
           dog2.setAge(2);
           System.out.println("姓名: " + dog2.getName() + "  年龄: " + dog2.getAge() + "  毛色: " + dog2.getColor());
       }
   }
   ```

   ```java
   
   public class Dog{
       private String name;//姓名
       private int age;//年龄
   
       public Dog(String name, int age, String color) {
           this.name = name;
           this.age = age;
           this.color = color;
       }
   
       public Dog() {
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public int getAge() {
           return age;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   
       public String getColor() {
           return color;
       }
   
       public void setColor(String color) {
           this.color = color;
       }
   
       private String color;//毛色
   
   
   }
   
   ```

   

