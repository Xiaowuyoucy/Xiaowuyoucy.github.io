---
title: IntelliJ IDEA 中自定义模板代码的缩写
date: 2020-02-04 05:58:59
tags:
categories: Java
doc:
---

# IntelliJ IDEA 中自定义模板代码的缩写     

### 方法一：新建 Live Template

step1.

点击 File – Setting

![](/images/javawz/ide/1203928-20190227160616868-1557469800.png)



 step2.
		选择 Live Template，点击右侧的+号，选择 Template Group 

![](/images/javawz/ide/1203928-20190227160645143-961396544.png)

 step3.
输入自定义的名称，然后点击OK。

 ![](/images/javawz/ide/1203928-20190227160709473-566828200.png)

 step4.
		选中刚创建的 Template Group，再次点击右侧的+号，选择Live Template 



![](/images/javawz/ide/1203928-20190227160722176-1574384979.png)

step5.
		填入缩写词、描述、模版内容、生效文件范围等

配置模板时，变量名以$ $包围的字符的形式出现,调用的方法也是先以变量形式出现，写完模板信息后去编辑变量(Edit variables)。

\$END\$是一个特殊的预定义变量，表示光标最后跳转的位置。

![](/images/javawz/ide/1203928-20190227160737420-1450085881.png)

step6.
点击右边的Edit variables（这个只有在使用了预定义函数的时候才能点击和设置）



 ![](/images/javawz/ide/1203928-20190227160755983-587139911.png)

step7.
点击 Apply ，OK，完成！

### 方法二：使用 IDEA 自带的 Live Template

psvm 生成 main 方法
fori 生成 for 循环
sout 生成 System.out.println();
…

### 方法三：修改 IDEA 自带的 Live Template

以将 psvm 修改成 main 和 sout 修改成 syso 为例：



![](/images/javawz/ide/1203928-20190227160848935-236915794.png)

![](/images/javawz/ide/1203928-20190227160905997-1365618359.png)