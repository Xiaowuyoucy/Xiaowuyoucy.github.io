---
title: JDK安装
date: 2020-02-04 04:59:05
tags:
categories: Java
doc:
---

java是oracle公司的,所以jdk要去oracle官网下载



安装路径中，不要包含中文和空格。

<br/>

<br/>

![image-20230910001325603](/images/javawz/image-20230910001325603.png)



<br/>

<br/>

![image-20230910001341784](/images/javawz/image-20230910001341784.png)



<br/>

<br/>

![image-20230910001347782](/images/javawz/image-20230910001347782.png)



<br/>

<br/>

![image-20230910001353350](/images/javawz/image-20230910001353350.png)

<br/>

<br/>

![image-20230910001400622](/images/javawz/image-20230910001400622.png)

<br/>

<br/>

### JAVA_HOME环境变量的配置

#### 配置环境变量作用

开发Java程序，需要使用JDK中提供的工具，工具在JDK9安装目录的`bin` 目录下。

![image-20230910001712630](/images/javawz/image-20230910001712630.png)



在DOS命令行下使用这些工具，就要先进入到JDK的bin目录下，这个过程就会非常的麻烦。


![image-20230910001728285](/images/javawz/image-20230910001728285.png)



不进入JDK的bin 目录，这些工具就不能使用，会报错。

![image-20230910001739257](/images/javawz/image-20230910001739257.png)



为了开发方便，我们想在任意的目录下都可以使用JDK的开发工具，则必须要配置环境变量，配置环境变量的意义
在于告诉操作系统，我们使用的JDK开发工具在哪个目录下。

#### 配置环境变量步骤

##### Windows 7,8版本

1. 计算机鼠标右键,选择`属性`

![image-20230910001823763](/images/javawz/image-20230910001823763.png)

2. 选择`高级系统设置`

![image-20230910001838511](/images/javawz/image-20230910001838511.png)



3. `高级`选项卡，点击`环境变量`


![image-20230910001859125](/images/javawz/image-20230910001859125.png)



4. 点击新建 ，创建新的环境变量


![image-20230910001912914](/images/javawz/image-20230910001912914.png)



5. 变量名输入`JAVA_HOME` ，变量值输入JDK9的安装目录 `c:\Java9\jdk-9.0.1`


![image-20230910001945183](/images/javawz/image-20230910001945183.png)

6. 选中`Path` 环境变量， 双击或者点击编辑


![image-20230910002005011](/images/javawz/image-20230910002005011.png)

7. 在变量值的最前面，键入%JAVA_HOME%\bin; 分号必须要写，必须是英文格式。

   ![image-20230910002035220](/images/javawz/image-20230910002035220.png)

8. 环境变量配置完成，重新开启DOS命令行，在任意目录下输入`javac` 命令，运行成功。

![image-20230910002100756](/images/javawz/image-20230910002100756.png)

<br/>

<br/>

<hr/><hr/>

<br/>

<br/>

<br/>



##### Windows 10 版本

<br/>

![image-20230910002134076](/images/javawz/image-20230910002134076.png)



![image-20230910002139446](/images/javawz/image-20230910002139446.png)





![image-20230910002145956](/images/javawz/image-20230910002145956.png)



![image-20230910002152109](/images/javawz/image-20230910002152109.png)



![image-20230910002156269](/images/javawz/image-20230910002156269.png)



![image-20230910002204046](/images/javawz/image-20230910002204046.png)

<br/>

<br/>

<br/><br/>

<br/>

<br/>