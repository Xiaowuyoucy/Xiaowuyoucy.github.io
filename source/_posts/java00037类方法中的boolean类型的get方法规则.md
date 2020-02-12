---
title: 类方法中的boolean类型的get方法规则
date: 2020-02-12 06:48:52
tags:
categories: Java
doc:
---

# 类方法中的boolean类型的get方法规则

##### &emsp;如果类中`boolean`类型的成员变量是`private`的,写`get方法`时的格式一定要是:

##### 以`is`开头,后面紧跟着变量名,变量名`首字符`一定要`大写`

public boolean isXxxx()

{

&emsp;&emsp;return (boolean类型变量);

}

##### 如果是``boolean`类型的成员变量是`private`,写`set`方法时,按普通写法:

public void setXxxx( boolean m_Xxxx)

{

&emsp;&emsp;Xxxx = m_Xxxx;

}

```java

public class Main02{
    public static void main(String[] args) {
        Student student = new Student();
        student.setName("小昭"); //设置姓名
        student.setAge(18);//设置年龄
        student.setMale(false);//设置性别

        System.out.println(student.getName());//获取姓名
        System.out.println(student.getAge());//获取年龄
        System.out.println("是否是男的: " + student.isMale());//获取性别

    }
}

```



```java

public class Student{
    private String name;//学生姓名
    private int age;//学生年龄
    private boolean male;//学生性别,是否是男的

    public String getName(){
        return name;
    }
    public int getAge(){
        return age;
    }
    public boolean isMale(){
        return male;
    }
    public void setName(String m_name){
        name = m_name;
    }
    public void setAge(int m_age)
    {
        age = m_age;
    }
    public void setMale(boolean m_setMale)
    {
        male = m_setMale;
    }

}

```

