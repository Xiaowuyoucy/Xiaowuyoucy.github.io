---
title: VS2017编译linux程序时出错的解决办法
date: 2022-05-31 07:53:34
tags:
categories: cpp
doc:
---

VS2017编译linux程序时，如果出错，一般会出现下面的错误提示信息：

```
g++ exited with code 1, please see the Output Window - Build output for more details (NOTE:
the build output verbosity might need to be changed in Tools Options to see more information in
the Output Window).
```

解决办法如下：

```
VS2017 -> 工具 -> 选项 -> 项目和解决方案 -> 生成并运行，将“MSBuild项目生成输出详细级别”和“MSBuild项目
生成日志文件详细级别”调整为“普通”，这样在编译linux程序时如果编译出错，可以看到详细的错误信息。
```

![image-20220531075434776](/images/javawz/image-20220531075434776.png)