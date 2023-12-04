---
title: Qt编程第二天
date: 2023-10-24 22:04:42
tags:
categories: Qt
doc:
---



### 菜单栏

```
//1、菜单栏 只能有一个
 QMenuBar * bar = menuBar();		//创建菜单栏
 setMenuBar(bar);		//将菜单栏添加到窗口

    //创建菜单
QMenu * fileMenu = bar->addMenu("文件");
QMenu * editMenu = bar->addMenu("编辑");

    //创建菜单项
QAction * newAction = fileMenu->addAction("新建");

    //添加分割线
fileMenu->addSeparator();

QAction * openAction = fileMenu->addAction("打开");
```



### 工具栏

```
//2、工具栏 可以有多个
    QToolBar * toolBar = new QToolBar(this);
    addToolBar(Qt::LeftToolBarArea,toolBar);

    //设置只允许左右停靠
    toolBar->setAllowedAreas(Qt::LeftToolBarArea | Qt::RightToolBarArea);

    //设置浮动
    toolBar->setFloatable(false);

    //设置移动 （总开关）
    toolBar->setMovable(false);

    //工具栏中放入小部件
    toolBar->addAction(newAction);
    //添加分割线
    toolBar->addSeparator();
    toolBar->addAction(openAction);
```





### 状态栏

```
    //状态栏  只能有一个
    QStatusBar * stBar = statusBar();
    setStatusBar(stBar);

    QLabel * label1 = new QLabel("左侧提示信息",this);
    stBar->addWidget(label1);

    QLabel * label2 = new QLabel("右侧提示信息",this);
    stBar->addPermanentWidget(label2);
```





### 铆接部件 （浮动窗口）

    //铆接部件 （浮动窗口）  可以有多个
    QDockWidget * dock = new QDockWidget("aaa" ,this);
    addDockWidget(Qt::BottomDockWidgetArea,dock);
    
     //只允许左右停靠
    dock->setAllowedAreas(Qt::LeftDockWidgetArea | Qt::RightDockWidgetArea);
###   核心部件  只能有一个

```
 //核心部件  只能有一个
    QTextEdit * edit = new QTextEdit(this); 
    setCentralWidget(edit); 	//将记事本设置成核心部件
```



```
#include "mainwindow.h"
#include <QMenuBar>
#include <QToolBar>
#include <QLabel>
#include <QStatusBar>
#include <QDockWidget>
#include <QTextEdit>
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{

    resize(600,400);

    //1、菜单栏 只能有一个
    QMenuBar * bar = menuBar();
    setMenuBar(bar);

    //创建菜单
    QMenu * fileMenu = bar->addMenu("文件");
    QMenu * editMenu = bar->addMenu("编辑");

    //创建菜单项
    QAction * newAction = fileMenu->addAction("新建");

    //添加分割线
    fileMenu->addSeparator();

    QAction * openAction = fileMenu->addAction("打开");


    //2、工具栏 可以有多个
    QToolBar * toolBar = new QToolBar(this);
    addToolBar(Qt::LeftToolBarArea,toolBar);

    //设置只允许左右停靠
    toolBar->setAllowedAreas(Qt::LeftToolBarArea | Qt::RightToolBarArea);

    //设置浮动
    toolBar->setFloatable(false);

    //设置移动 （总开关）
    toolBar->setMovable(false);

    //工具栏中放入小部件
    toolBar->addAction(newAction);
    //添加分割线
    toolBar->addSeparator();
    toolBar->addAction(openAction);

    //状态栏  只能有一个
    QStatusBar * stBar = statusBar();
    setStatusBar(stBar);

    QLabel * label1 = new QLabel("左侧提示信息",this);
    stBar->addWidget(label1);

    QLabel * label2 = new QLabel("右侧提示信息",this);
    stBar->addPermanentWidget(label2);



    //铆接部件 （浮动窗口）  可以有多个
    QDockWidget * dock = new QDockWidget("aaa" ,this);
    addDockWidget(Qt::BottomDockWidgetArea,dock);

    //只允许左右停靠
    dock->setAllowedAreas(Qt::LeftDockWidgetArea | Qt::RightDockWidgetArea);

    //核心部件  只能有一个
    QTextEdit * edit = new QTextEdit(this);
    setCentralWidget(edit);
}



MainWindow::~MainWindow()
{

}

```



<br/><br/>

### UI界面设计

这里面不能输入中文

![image-20231025222114426](/images/javawz/image-20231025222114426.png)



输入英文后再这里可以修改为中文

![image-20231025222220405](/images/javawz/image-20231025222220405.png)



<br/><br/>

### 资源文件

添加资源文件

![img](/images/javawz/wps1.jpg)



![img](/images/javawz/wps2.jpg)

打开资源文件需要鼠标右键选择 Open in Editor

![image-20231025011538366](/images/javawz/image-20231025011538366.png)





![img](/images/javawz/wps3-1698167691853.jpg)





添加前缀用以区分资源文件,相当于文件夹

![image-20231025011639424](/images/javawz/image-20231025011639424.png)



![image-20231025011612121](/images/javawz/image-20231025011612121.png)



添加文件

![image-20231025011704071](/images/javawz/image-20231025011704071.png).





如果上面添加不了,可以用鼠标右键选择对应的选项来添加

![image-20231025011853479](/images/javawz/image-20231025011853479.png)





```
资源文件添加   语法：   ": + 前缀名  + 文件名称"
```



```
#include "mainwindow.h"
#include "ui_mainwindow.h"

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);


    //给新建添加小图标
    //ui->actionNew->setIcon(QIcon("E:/Image/Luffy.png"));

    //资源文件添加   语法：   ": + 前缀名  + 文件名称"
    ui->actionNew->setIcon(QIcon(":/Image/Luffy.png"));

    ui->actionOpen->setIcon(QIcon(":/Image/LuffyQ.png"));
}

MainWindow::~MainWindow()
{
    delete ui;
}

```



<br/><br/><br/><br/>

### 对话框

#### 模态对话框创建

```
//模态对话框创建
//        QDialog dlg(this);
//        dlg.resize(120,30);
//        dlg.exec();
```

#### 非模态对话框

```
//非模态对话框创建
//          QDialog *dlg2 = new QDialog(this);
//          dlg2->resize(120,30);
//          dlg2->show();
//          //设置 55号属性
//          dlg2->setAttribute(Qt::WA_DeleteOnClose);
```



#### QMessageBox对话框

```
//错误提示对话框
//QMessageBox::critical(this,"错误","critical");

//信息提示对话框
//QMessageBox::information(this,"信息","info");

//询问提示对话框
 // 参数1   父窗口  参数2  标题  参数3  提示信息  参数4  按键类型  参数5  默认关联回车按键
//        if( QMessageBox::Save ==  QMessageBox::question(this,"询问","question" , QMessageBox::Save | QMessageBox::Cancel ,QMessageBox::Cancel))
//        {
//             qDebug()<<"点击的是保存";
//        }
//        else
//        {
//             qDebug()<<"点击的是取消";
//        }


//警告提示对话框
//QMessageBox::warning(this,"警告","warning");
```



#### 标准常用的对话框

```
QColorDialog：			选择颜色；
QFileDialog：			选择文件或者目录；
QFontDialog：			选择字体；
QInputDialog：			允许用户输入一个值，并将其值返回；
QMessageBox：			模态对话框，用于显示信息、询问问题等；
QPageSetupDialog：		为打印机提供纸张相关的选项；
QPrintDialog：			打印机配置；
QPrintPreviewDialog：	打印预览；
QProgressDialog：		显示操作过程。
```



<br/><br/>

```
//颜色对话框
//         QColor color = QColorDialog::getColor(Qt::red);
//         qDebug() << color.red() << color.green() << color.blue() ;

//文件对话框
//          QString fileName = QFileDialog::getOpenFileName(this,"打开文件","C:\\Users\\zhangtao\\Desktop","(*.doc)");
//          qDebug () <<fileName;

//字体对话框
        bool flag;
        QFont font = QFontDialog::getFont(&flag,QFont("华文彩云",36));
        qDebug() << "字体" << font.family().toUtf8().data() << "字号"<< font.pointSize()
                 << "是否加粗"<<font.bold() << "是否倾斜" << font.italic();
```





### 界面布局

#### 标签

![image-20231107221304796](/images/javawz/image-20231107221304796.png)



<br/><br/>

#### 单行编辑框

![image-20231107221411172](/images/javawz/image-20231107221411172.png)

echoMode属性

![image-20231107224230784](/images/javawz/image-20231107224230784.png)

1. **Normal（正常模式）**：这是默认模式，用户输入的文本以明文形式显示在编辑框中。这是最常见的使用方式，适用于大多数文本输入场景。
2. **NoEcho（无回显模式）**：在这个模式下，用户输入的文本不会显示在编辑框内，用于隐藏用户输入的文本，例如用于密码输入，以保护敏感信息。
3. **Password（密码模式）**：在这个模式下，用户输入的文本以密码掩码字符（通常是圆点或星号）显示在编辑框内。这是用于密码输入框的常见设置，以保护密码的机密性。
4. **PasswordEchoOnEdit（编辑时密码模式）**：用户在输入时，文本以明文形式显示在编辑框内，但当编辑框失去焦点后，文本会以密码掩码字符显示。这是一种用户友好的密码输入方式，让用户能够确认他们输入的内容。

<br/><br/>

#### 容器

![image-20231107221630854](/images/javawz/image-20231107221630854.png)

 容器属性:

​				固定容器大小



<br/><br/>

![image-20231107223351226](/images/javawz/image-20231107223351226.png)

#### 水平布局

![image-20231107222105903](/images/javawz/image-20231107222105903.png)

<br/><br/>

#### 垂直布局

![image-20231107222141029](/images/javawz/image-20231107222141029.png)

<br/><br/>

#### 栅格布局

几行几列的选栅格布局,然后再选垂直布局

![image-20231107222937304](/images/javawz/image-20231107222937304.png)



没有布局会有红圈

![image-20231107222017658](/images/javawz/image-20231107222017658.png)



<br/><br/>

#### 弹簧

固定布局的

![image-20231107222349302](/images/javawz/image-20231107222349302.png)



##### 弹簧属性

![image-20231107222510511](/images/javawz/image-20231107222510511.png)

Fixed是固定弹簧大小

Expanding是可伸缩的





<br/><br/>

#### 打破布局

![image-20231107222718372](/images/javawz/image-20231107222718372.png)



![image-20231107222802528](/images/javawz/image-20231107222802528.png)



<br/><br/>

#### 修改窗口标题

![image-20231107223711556](/images/javawz/image-20231107223711556.png)

<br/><br/>

#### 修改容器内边距

![image-20231107223909831](/images/javawz/image-20231107223909831.png)

![image-20231107223938922](/images/javawz/image-20231107223938922.png)

<br/><br/>

#### 固定窗口大小

![image-20231107224614856](/images/javawz/image-20231107224614856.png)

把minimumSize和maximumSize值都设置一样





### 常用控件--按钮组





































































































