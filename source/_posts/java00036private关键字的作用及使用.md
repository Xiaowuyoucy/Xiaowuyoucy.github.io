---
title: private关键字的作用及使用
date: 2020-02-11 18:51:53
tags:
categories: Java
doc:
---

# private关键字的作用及使用

#### 问题描述:定义Person的年龄时,无法阻止不合理的数值被赋值进来

#### 解决方案:用`private`关键字将需要保护的成员变量进行修饰.

##### &emsp;&emsp;   `private 数据类型 变量名`

#### 但是!超出了本类范围之外就不能再直接访问了.

##### private成员变量的获取和设置方法规范格式:

##### 获取:`getXxxx`开头,小驼峰式法,Xxxx代表私有的成员变量名

##### 设置:`setXxxx`开头,小驼峰式法,Xxxx代表私有的成员变量名

public void setXxxx(int a)

{

​	xxxx = a;

}

public int getXxxx()

{

return xxxx;

}

```java

public class Main01{
    public static void main(String[] args) {
        Person  person = new Person();
        person.name = "张无忌";
        person.setAge(18);
        person.setHeight(179);
        person.setSex("男");
        person.showPersonAll();
    }
}

```

```java
package xiaochenyan.top.person;

public class Person{

    String name;
    private int age; //私有成员变量
    private int height; //私有成员变量
    private String sex; //私有成员变量
	//用于设置age数据
    public void setAge(int m_age)
    {
        if(m_age < 0)
        {
            System.out.println("数据异常");
            return;
        }
        age = m_age;
    }
    //用于设置height数据
    public void setHeight(int m_height)
    {
        if(m_height < 0)
        {
            System.out.println("数据异常");
            return;
        }
        height = m_height;

    }
	//用于设置sex数据
    public void setSex(String m_sex)
    {

        sex = m_sex;
    }
    //用于获取name数据
    public String getName()
    {
        return name;
    }
        //用于获取sex数据
    public String getSex()
    {
        return sex;
    }
     //用于获取age数据
    public int getAge(){
        return age;
    }
    //用于获取height数据
    public  int getHeight()
    {
        return height;
    }
        //显示所有的成员变量
    public void showPersonAll()
    {
        System.out.println(name);
        System.out.println(age);
        System.out.println(height);
        System.out.println(sex);
    }

}

```

