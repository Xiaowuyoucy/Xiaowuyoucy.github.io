---
title: Java编译和运行
date: 2020-02-03 11:22:30
tags:
categories: Java
doc:
---

# <div id="kt">目录:</div>

##### <a style="text-decoration: none;color:black;" href="#md1" > 源文件后缀名</a>

##### <a style="text-decoration: none;color:black;" href="#md2">编写java源文件模板</a>

##### <a style="text-decoration: none;color:black;" href="#md3">编译和运行</a>

##### <a style="text-decoration: none;color:black;" href="#md4">注释</a>

##### <a style="text-decoration: none;color:black;" href="#md5">关键字</a>

##### <a style="text-decoration: none;color:black;" href="#md6">标识符</a>

##### <a style="text-decoration: none;color:black;" href="#md7">常量</a>

##### <a style="text-decoration: none;color:black;" href="#md8">数据类型</a>

------



### <a style="text-decoration: none;color:black;" href="#kt"> <div id="md1">源文件后缀名</div> </a>

java的源文件后缀名为  *.java

------

### <a style="text-decoration: none;color:black;" href="#kt"><div id="md2">编写java源文件模板</div></a>

```java
public filename{	
    public static void main(String [] args)
	{
		System.out.println("Hello World.....");
	}
}
```

其中filename对应着源文件名

System.out.println为输出函数 ,输出内容,结尾时换行

------

### <a style="text-decoration: none;color:black;" href="#kt"><div id="md3">编译和运行</div></a>

编译　javac 文件名　　　　编译成功会生成一个class文件

运行　java 文件名　　　　　文件名后面不要带class后缀

------

### <a style="text-decoration: none;color:black;" href="#kt"><div id="md4">注释</div></a>

// 单行注释

/**/多行注释

```java
public filename{	
    public static void main(String [] args)
	{
		//我是单行注释
        
        
        /*	
        
        	我是多行注释
        	我是多行注释
        	
        */
	}
}
```

------

### <a style="text-decoration: none;color:black;" href="#kt"><div id="md5">关键字</div></a>

- ​	特征
  - ​		全部小写
  - ​	在增强版记事本中有颜色

------

### <a style="text-decoration: none;color:black;" href="#kt"><div id="md6">标识符</div></a>

- ##### 	命名规则

  - 可以包含字母，数字，下划线，$
  - 不能以数字开头
  - 标识符不能是关键字

- ##### 　命名规范

  - 类名　　大驼峰式 :首字母大写，后面每个单词首字母大写 MyNmae
  - 变量名　　小驼峰式：首字母小写，后面每个单词首字母大写nameSize
  - 方法名　　小驼峰式：首字母小写，后面每个单词首字母大写nameSize

------

### <a style="text-decoration: none;color:black;" href="#kt"><div id="md7">常量</div></a>

程序运行期间，固定不变的量

```java
public filename{
    public static void mian(String[] args)
    {
        String a = "abc";			//字符串常量
        
        int iZhengXing = 123;		//整型常量
        
        float fXiaoShu = 123.2F;		//浮点型常量
        
        char cZifu = 'c';			//字符常量
        
        boolean isBool = true;		//布尔型常量
        
        //还有一个null  空常量
        
    }
}
```

------

### <a style="text-decoration: none;color:black;" href="#kt"><div id="md8">数据类型</div></a>

#### 	整型

byte，　short，　int，　long

#### 	浮点型

​		float，double

#### 	布尔型

​		boolean

#### 	字符型

​		char
​	

```java
public filename{
    public static void mian(String[] args)
    {
       /***********整型************/
       byte 	bXinBie;		//1个字节
       short	sXiaoZheng;		//2个字节
       int		iRenLei;		//4个字节
       long		lShaZi;			//4个字节
        
        /**********浮点型************/
        float 	fFengShu;			//4个字节
        double 	dYuanZhouLv;		//8个字节
        
        /**********布尔型************/
        boolean isOk;			
        
        /**********字符型************/
        char 	cPingFen;			//1个字节
    }
}
```



#### 注意事项

1. ​		字符串不是基本类型，而是引用类型
2. ​		浮点型可能只是一个近似值，并非精确值
3. ​		数据范围与字节数不一定相关，例如float数据比long更加广泛，但是float更加广泛
4. ​		浮点数默认是double类型，如果要用float类型要加后缀F
5. ​		整数默认类型是int，如果要用long，后缀要加L