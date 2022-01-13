---
title: 软件的安装和卸载
date: 2022-01-07 23:56:39
tags:
categories: linux
doc:
---

## 在线安装

#### `    apt-get`

​	安装 

```
sudo apt-get install 软件名
```

​	卸载 

```
sudo apt-get remove  软件名
```

​	更新 

```
sudo apt-get update  更新软件列表
```

​	软件列表存放的是软件名字和下载地址

清理所有软件安装包

```
sudo apt-get clean
```

实际清理的是: /var/cache/apt/archives目录下的.deb文件

<br/>

#### `aptitude `

```
安装 ：sudo apt-get install aptitude
sudo aptitude show 软件名		//查看软件是否安装
```

安装：

```
sudo aptitude install 软件名
```

重新安装：

```
sudo aptitude reinstall 软件名
```

更新：

```
 sudo apt-get update
```

移除：

```
sudo aptitude rermove 软件名
```

显示状态：

```
 sudo aptitude show 软件名
```

<br/><br/><br/>

## deb包安装

安装 

```
sudo dpkg -i xxx.deb
```

卸载 

```
sudo dpkg -r xxx
```

<br/><br/><br/>

## 源码安装

```
1.解压缩源代码包
2.进入到安装目录
3.检测文件是否缺失,创建Makefile,检测编译环境: ./configure
4.编译源码 生成库和可以执行程序:make
5.把库和可执行程序,安装到系统目录下: sudo make install
6.删除和卸载软件:sudo make distclean
7.上述安装步骤并不是绝对的,应该先查看附带的README文件
```

<br/><br/><br/>