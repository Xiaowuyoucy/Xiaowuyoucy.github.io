---
title: Debug追踪
date: 2020-04-06 00:46:44
tags:
categories: Java
doc:
---

# Debug追踪

**使用IDEA的断点调试功能，查看程序的运行过程**



### Debug调试程序:

可以让代码逐行执行,查看代码执行的过程,调试程序中出现的bug
使用方式:
在行号的右边,鼠标左键单击,添加断点(每个方法的第一行,哪里有bug添加到哪里)
右键,选择Debug执行程序
程序就会停留在添加的第一个断点处
执行程序:
f8:逐行执行程序
f7:进入到方法中
shift+f8:跳出方法
f9:跳到下一个断点,如果没有下一个断点,那么就结束程序
ctrl+f2:退出debug模式,停止程序
Console:切换到控制台

1. 在有效代码行，点击行号右边的空白区域，设置断点，程序执行到断点将停止，我们可以手动来运行程序 	

   ![](/images/javawz/debug1.png)

2. 点击Debug运行模式 

   ​     ![](/images/javawz/debug2.png)                                                                                                                                                                      

3.  程序停止在断点上不再执行，而IDEA最下方打开了Debug调试窗口  

     ![](/images/javawz/debug3.png)![](/images/javawz/debug4.png)

4. Debug调试窗口介绍

   ![](/images/javawz/debug5.png)

5. 快捷键F8，代码向下执行一行,第九行执行完毕，执行到第10行（第10行还未执行）

   ![](/images/javawz/debug6.png)

6. 切换到控制台面板，控制台显示 请录入一个字符串： 并且等待键盘录入

   ![](/images/javawz/debug7.png)

7. 快捷键F8，程序继续向后执行，执行键盘录入操作，在控制台录入数据 ababcea

   ![](/images/javawz/debug8.png)

   回车之后效果：

   ![](/images/javawz/debug9.png)

   调试界面效果：

   ![](/images/javawz/debug0.png)

8. 此时到达findChar方法，快捷键F7，进入方法findChar

   ![](/images/javawz/debug11.png)

9. 快捷键F8 接续执行，创建了map对象，变量区域显示

   ![](/images/javawz/debug12.png)

10. 快捷键F8 接续执行，进入到循环中，循环变量i为 0,F8再继续执行，就获取到变量c赋值为字符‘a’ 字节值97

    ![](/images/javawz/debug13.png)

11. 快捷键F8 接续执行，进入到判断语句中，因为该字符 不在Map集合键集中，再按F8执行，进入该判断中

    ![](/images/javawz/debug14.png)

12. 快捷键F8 接续执行，循环结束，进入下次循环，此时map中已经添加一对儿元素

    ![](/images/javawz/debug15.png)

13. 快捷键F8 接续执行，进入下次循环，再继续上面的操作，我们就可以看到代码每次是如何执行的了

    ![](/images/javawz/debug16.png)

14. 如果不想继续debug,那么可以使用快捷键F9,程序正常执行到结束，程序结果在控制台显示

    ![](/images/javawz/debug17.png)

#