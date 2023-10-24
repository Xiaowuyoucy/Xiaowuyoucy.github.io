---
title: Qt编程第一天
date: 2023-10-11 23:09:49
tags:
categories: Qt
doc:
---



### .pro文件

```
QT       += core gui  //包含的模块
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets //大于Qt4版本 才包含widget模块
TARGET = QtFirst  //应用程序名  生成的.exe程序名称
TEMPLATE = app    //模板类型    应用程序模板
SOURCES += main.cpp\   //源文件
        mywidget.cpp
HEADERS  += mywidget.h   //头文件
```





**.pro就是工程文件(project)，它是qmake自动生成的用于生产makefile的配置文件**。.pro文件的写法如下：

#### 注释

从“#”开始，到这一行结束。



#### 模板变量

告诉qmake为这个应用程序生成哪种makefile。下面是可供使用的选择：**TEMPLATE** = app

app -建立一个应用程序的makefile。这是默认值，所以如果模板没有被指定，这个将被使用。

lib - 建立一个库的makefile。

vcapp - 建立一个应用程序的VisualStudio项目文件。

vclib - 建立一个库的VisualStudio项目文件。

subdirs -这是一个特殊的模板，它可以创建一个能够进入特定目录并且为一个项目文件生成makefile并且为它调用make的makefile。



#### 配置信息

CONFIG用来告诉qmake关于应用程序的配置信息。

CONFIG += c++11	//使用c++11的特性

在这里使用“+=”，是因为我们添加我们的配置选项到任何一个已经存在中。这样做比使用“=”那样替换已经指定的所有选项更安全。





<hr>

### 命名规范

类名：首字母 大小  单词和单词之间 首字母 大写
变量、函数名： 首字母小写 单词和单词之间 首字母 大写



### 快捷键

```
运行 ctrl + R
编译 ctrl + B
查询 ctrl + F
注释 ctrl + /
帮助 F1
字体缩放  ctrl + 鼠标滚轮
整行代码移动  ctrl + shift + ↑ ↓
自动对齐  ctrl + i
同名之间的.h .cpp切换  F4

帮助文档 F1    左侧列表中按钮    C:\Qt\Qt5.6.0\5.6\mingw49_32\bin
```



### QPushButton基本创建



mywidget.h

```
#ifndef MYWIDGET_H
#define MYWIDGET_H

#include <QWidget>

class MyWidget : public QWidget
{
    Q_OBJECT  //Q_OBJECT宏  支持信号和槽

public:
    MyWidget(QWidget *parent = 0);
    ~MyWidget();
};

#endif // MYWIDGET_H

```



mywidget.cpp

```cpp
#include "mywidget.h"
#include <QPushButton>
#include "mypushbutton.h"
#include <QDebug>
//命名规范
// 类名 首字母 大小  单词和单词之间 首字母 大写
// 变量、函数名 首字母小写 单词和单词之间 首字母 大写

//快捷键
// 运行 ctrl + R
// 编译 ctrl + B
// 查询 ctrl + F
// 注释 ctrl + /
// 帮助 F1
// 字体缩放  ctrl + 鼠标滚轮
// 整行代码移动  ctrl + shift + ↑ ↓
// 自动对齐  ctrl + i
// 同名之间的.h .cpp切换  F4

// 帮助文档 F1    左侧列表中按钮    C:\Qt\Qt5.6.0\5.6\mingw49_32\bin


MyWidget::MyWidget(QWidget *parent)
    : QWidget(parent)
{
    //按钮
    QPushButton * btn = new QPushButton;

    //btn->show(); //show用顶层方式弹出

    //如果想显示到当前窗口中 ，需要做依赖,设置父亲
    btn->setParent(this);

    //显示文本
    btn->setText("德玛西亚");

    //按钮2
    QPushButton * btn2 = new QPushButton("第二个",this);

    //移动btn2
    btn2->move(100,100);

    //重置窗口大小
    resize(600,400);

    //按钮 可以重置大小吗？ 可以
    //btn->resize(300,200);

    //指定窗口标题
    setWindowTitle("第一个窗口");

    //设置窗口固定大小
    setFixedSize(600,400);


    //创建自定义的按钮
    MyPushButton * myBtn  = new MyPushButton;
    myBtn->setParent(this);
    myBtn->setText("我的按钮");
    myBtn->move( 300,200);


    //点击按钮  关闭窗口
    //connect(  信号发送者，发送的信号，信号的接受者，处理的槽函数）
    //信号和槽 优点： 松散耦合
   // connect(myBtn, &QPushButton::clicked , this, &QWidget::close );
    connect( myBtn , &MyPushButton::clicked ,this, &MyWidget::close);

}

MyWidget::~MyWidget()
{
    qDebug() << "MyWidget析构调用";
}

```



mypushbutton.h

```cpp
#ifndef MYPUSHBUTTON_H
#define MYPUSHBUTTON_H

#include <QPushButton>

//这个类作用于什么,就继承什么
class MyPushButton : public QPushButton
{
    Q_OBJECT
public:
    explicit MyPushButton(QWidget *parent = 0);

    ~MyPushButton();

signals:

public slots:
};

#endif // MYPUSHBUTTON_H

```



mypushbutton.cpp

```cpp
#include "mypushbutton.h"
#include <QDebug>
MyPushButton::MyPushButton(QWidget *parent) : QPushButton(parent)
{

}

MyPushButton::~MyPushButton()
{
    qDebug() << "MyPushButton的析构函数调用";
}

```





### 对象模型（对象树）

一定程度上简化了内存回收机制

当创建的对象 指定的父亲是由QObject或者Object派生的类时候，这个对象被加载到对象树上，当窗口关闭掉时候，树上的对象也都会被释放掉



### Qt中的坐标系

x以右侧为正

y以下侧为正

左上角是 0,0点





### Qt中信号和槽基本使用

![Qt信号和槽 ](/images/javawz/Qt信号和槽 .png)

![image-20231017231902044](/images/javawz/image-20231017231902044.png)

需求：点击按钮关闭窗口

连接  connect ( 信号的发送者，发送的信号，信号的接受者，处理的槽函数)

```
connect( myBtn , &MyPushButton::clicked ,this, &MyWidget::close);
```



当自定义插槽类有多个信号时,要使用函数指针来传递给connect函数,否则会出现歧义性,导致出错。

```
#include "mywidget.h"
#include <QPushButton>
#include "mypushbutton.h"
#include <QDebug>
//命名规范
// 类名 首字母 大小  单词和单词之间 首字母 大写
// 变量、函数名 首字母小写 单词和单词之间 首字母 大写

//快捷键
// 运行 ctrl + R
// 编译 ctrl + B
// 查询 ctrl + F
// 注释 ctrl + /
// 帮助 F1
// 字体缩放  ctrl + 鼠标滚轮
// 整行代码移动  ctrl + shift + ↑ ↓
// 自动对齐  ctrl + i
// 同名之间的.h .cpp切换  F4

// 帮助文档 F1    左侧列表中按钮    C:\Qt\Qt5.6.0\5.6\mingw49_32\bin


MyWidget::MyWidget(QWidget *parent)
    : QWidget(parent)
{
    //按钮
    QPushButton * btn = new QPushButton;

    //btn->show(); //show用顶层方式弹出

    //如果想显示到当前窗口中 ，需要做依赖
    btn->setParent(this);

    //显示文本
    btn->setText("德玛西亚");

    //按钮2
    QPushButton * btn2 = new QPushButton("第二个",this);

    //移动btn2
    btn2->move(100,100);

    //重置窗口大小
    resize(600,400);

    //按钮 可以重置大小吗？ 可以
    //btn->resize(300,200);

    //指定窗口标题
    setWindowTitle("第一个窗口");

    //设置窗口固定大小
    setFixedSize(600,400);


    //创建自定义的按钮
    MyPushButton * myBtn  = new MyPushButton;
    myBtn->setParent(this);
    myBtn->setText("我的按钮");
    myBtn->move( 300,200);


    //点击按钮  关闭窗口
    //connect(  信号发送者，发送的信号，信号的接受者，处理的槽函数）
    //信号和槽 优点： 松散耦合
   // connect(myBtn, &QPushButton::clicked , this, &QWidget::close );
    connect( myBtn , &MyPushButton::clicked ,this, &MyWidget::close);

}

MyWidget::~MyWidget()
{
    qDebug() << "MyWidget析构调用";
}

```

如果一个类找不到继承,可以选择QObject类

student.h

```
#ifndef STUDENT_H
#define STUDENT_H

#include <QObject>

class Student : public QObject
{
    Q_OBJECT
public:
    explicit Student(QObject *parent = 0);

signals:


    //自定义槽函数 写到public slots  Qt 5.0版本以上 可以写成全局函数或者public作用域下 或者 lambda表达式
public slots:

    //返回值是void
    //需要声明 也需要有实现
    //可以有参数  可以发生重载
    void treat();


    void treat(QString foodName);
};

#endif // STUDENT_H

```



student.cpp

```
#include "student.h"
#include <QDebug>
Student::Student(QObject *parent) : QObject(parent)
{

}


void Student::treat()
{
    qDebug() << "请老师吃饭";
}


void Student::treat(QString foodName)
{
	//如果打印QString类型的字符串会自带双引号,除非转换为chat * 类型
    //QString 转 char *    通过.toUtf8转为 QByteArray 类型  通过 .data()转为 char *

     qDebug() << "请老师吃饭 , 老师要吃： " << foodName.toUtf8().data();
}

```



teacher.h

```
#ifndef TEACHER_H
#define TEACHER_H

#include <QObject>

class Teacher : public QObject
{
    Q_OBJECT
public:
    explicit Teacher(QObject *parent = 0);

//自定义信号  写到signals下

signals:
    //返回值是void
    //只需要声明 不需要实现
    //可以有参数  可以发生重载
    void hungry();

    void hungry( QString foodName);

public slots:
};

#endif // TEACHER_H

```

```
#include "teacher.h"

Teacher::Teacher(QObject *parent) : QObject(parent)
{

}

```





widget.h

```
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include "student.h"
#include "teacher.h"

class Widget : public QWidget
{
    Q_OBJECT

public:
    Widget(QWidget *parent = 0);
    ~Widget();


    Teacher * zt;
    Student * st;


    //下课
    void classIsOver();
};

#endif // WIDGET_H

```



widget.cpp

```
#include "widget.h"
#include <QPushButton>
#include <QDebug>
// Teacher老师类
// Student学生类
// 下课后  老师会触发一个 饿了 的信号
// 学生响应这个信号 并且 请老师吃饭



Widget::Widget(QWidget *parent)
    : QWidget(parent)
{
	//参数带this代表窗口关闭会执行析构函数
    this->zt = new Teacher(this);
    this->st = new Student(this);

    //连接信号和槽
    //connect(zt,&Teacher::hungry,st,&Student::treat);

    //classIsOver();


    //连接有参信号和槽
    // 函数指针 可以指向 函数地址
//    void(Teacher:: *teacherSignal)(QString) = &Teacher::hungry;

//    void(Student:: *studentSlot)(QString ) = &Student::treat;

//    connect(zt,teacherSignal, st,studentSlot);

   // classIsOver();

    //创建按钮
    QPushButton * btn = new QPushButton("下课" , this);

    resize(600,400);


    void(Teacher:: *teacherSignal2)() = &Teacher::hungry;

    void(Student:: *studentSlot2)() = &Student::treat;

    connect(zt,teacherSignal2, st,studentSlot2);

    //1、信号是可以连接信号
    connect(btn,&QPushButton::clicked,zt,teacherSignal2);
    //2、可以断开信号和槽
    disconnect(zt,teacherSignal2, st,studentSlot2);

    //3、一个信号可以响应多个槽函数

    //4、多个信号可以连接同一个槽函数

    //5、信号和槽函数的参数类型 必须一一对应
    //   信号的参数个数 可以多余槽函数的参数个数，反之不可以 , 参数类型要一一对应


    //Qt4版本信号和槽写法
    //利用Qt4版本连接有参信号和槽
    //优势 ：参数直观
    //劣势 ：参数类型不做匹配检测
    // Qt4本质   SIGNAL("hungry(int)")SLOT("treat(QString)")
//    connect(zt, SIGNAL(hungry(QString)) , st , SLOT(treat(QString)));

//    classIsOver();


    //[=] 函数体内可以使用Lambda所在作用范围内所有可见的局部变量
    QPushButton * btn2 = new QPushButton("aaa",this);
    QPushButton * btn3 = new QPushButton("aaa",this);
    [=](){
        btn2->setText("bbb");
        btn3->setText("bbb");
    }();

    //最常用lambda使用 [=](){}
    QPushButton * btn4 = new QPushButton("aaa",this);
    btn4->move( 100, 0);

    //当进行信号和槽连接时候，控件内会进入一个锁的状态
    connect(btn4,&QPushButton::clicked,this,[=](){
        btn4->setText("bbb");
    });


   //加上mutable修饰符后，可以修改按值传递进来的拷贝
   QPushButton * myBtn = new QPushButton (this);
   QPushButton * myBtn2 = new QPushButton (this);
   myBtn2->move(100,100);
   int m = 10;

   connect(myBtn,&QPushButton::clicked,this,[m] () mutable { m = 20; qDebug() << m; });

   connect(myBtn2,&QPushButton::clicked,this,[=] ()  { qDebug() << m; });

   qDebug() << m;


   //-> 返回值类型
   int num = [=]()->int{
        return 1000;
   }();
   qDebug() << "num = " << num ;


   //点击按钮 关闭窗口
   connect(btn4,&QPushButton::clicked,[=](){
        //this->close();
        st->treat("宫保鸡丁");
   });

}

void Widget::classIsOver()
{
    //触发自定义信号
//    emit this->zt->hungry();


   emit this->zt->hungry("宫保鸡丁");
}

Widget::~Widget()
{

}

```



### Lambda表达式

C++11中的Lambda表达式**用于定义并创建匿名的函数对象**，以简化编程工作。首先看一下Lambda表达式的基本构成：

```
[capture](parameters) mutable ->return-type
{
statement
}

[函数对象参数](操作符重载函数参数)mutable ->返回值{函数体}
```



函数对象参数有以下形式：
空。没有使用任何函数对象参数。

<br/>

= ：函数体内可以使用Lambda所在作用范围内所有可见的局部变量（包括Lambda所在类的this），并且是值传递方式（相当于编译器自动为我们按值传递了所有局部变量）。

<br/>

& ：函数体内可以使用Lambda所在作用范围内所有可见的局部变量（包括Lambda所在类的this），并且是引用传递方式（相当于编译器自动为我们按引用传递了所有局部变量）。this。函数体内可以使用Lambda所在类中的成员变量。

<br/>

a ：将a按值进行传递。按值进行传递时，函数体内不能修改传递进来的a的拷贝，因为默认情况下函数是const的。要修改传递进来的a的拷贝，可以添加mutable修饰符。

<br/>

&a ：将a按引用进行传递。

<br/>

a, &b ：将a按值进行传递，b按引用进行传递。

<br/>

=，&a, &b ：除a和b按引用进行传递外，其他参数都按值进行传递。

<br/>

&, a, b ：除a和b按值进行传递外，其他参数都按引用进行传递。

<br/>

 操作符重载函数参数；
标识重载的()操作符的参数，没有参数时，这部分可以省略。参数可以通过按值（如：(a,b)）和按引用（如：(&a,&b)）两种方式进行传递。

<br/>

可修改标示符；
mutable声明，这部分可以省略。按值传递函数对象参数时，加上mutable修饰符后，可以修改按值传递进来的拷贝（注意是能修改拷贝，而不是值本身）。

```
QPushButton * myBtn = new QPushButton (this);
QPushButton * myBtn2 = new QPushButton (this);
myBtn2->move(100,100);
int m = 10;

connect(myBtn,&QPushButton::clicked,this,[m] ()mutable { m = 20; qDebug() << m; });

connect(myBtn2,&QPushButton::clicked,this,[=] ()  { qDebug() << m; });

qDebug() << m;
```



函数返回值；

`->`返回值类型，标识函数返回值的类型，当返回值为void，或者函数体中只有一处return的地方（此时编译器可以自动推断出返回值类型）时，这部分可以省略。

{}是函数体；

​	`{}`，标识函数的实现，这部分不能省略，但函数体可以为空。











