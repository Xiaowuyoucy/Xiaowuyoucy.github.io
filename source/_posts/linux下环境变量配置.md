---
title: linux下环境变量配置
date: 2022-05-16 01:08:36
tags:
categories: linux
doc:
---

### 环境变量

1. [环境变量的分类]()
2. [查看环境变量]()
3. [常用命名]()
   1. [PATH]()
   2. [LANG]()
   3. [HOSTNAME]()
   4. [SHELL]()
   5. [HISTSIZE]()
   6. [USER]()
   7. [HOME]()
   8. [PWD]()
   9. [CLASSPATH]()

4. [设置环境变量]()

   1. [系统环境变量]()
      1. [在/etc/profile文件中设置]()
      2. [在/etc/profile文件目录中增加环境变量脚本文件,这是linux推荐的方法]()
      3. [在/etc/bashrc文件中设置环境变量]()

   2. [用户环境变量]()
      1. [ .bash_profile(推荐首选)]()
      2. [ .bashrc]()
      3. [ .bash_logout]()
      4. [.bash_history]()

5. [重要的环境变量]()
   1. [PATH环境 变量]()
   2. [LANG环境变量]()
   3. [CLASSPATH]()

linux环境变量也成为Shell环境变量，习惯大写。
<br/><br/>

## 1.环境变量的分类

- 按生效的范围分类
  - 系统环境变量
    - 公共的，对全部的用户都生效
  - 用户环境变量     
    - 用户私有的、自定义的个性化设置、只针对该用户有效
- 按生存周期分类
  - 永久环境变量     
    - 在环境变量脚本文件中配置，用户每次登录时会自动执行这些脚本，相当于永久有效
  - 临时环境变量     
    - 使用时在Shell中临时定义，退出Shell后失效。

<br/><br/>

## 2.查看环境变量

```
env //查看所有环境变量

env | grep lan //用lan过滤环境变量



echo $PATH  //查看变量值
```

<br/><br/>

## 3.常用命名

#### 1.PATH

path为可执行环境的搜索目录，可执行程序包括Linux系统命令和用户的应用程序

#### 2.LANG

#### 3.HOSTNAME

服务器的主机名

#### 4.SHELL

用户当前使用的Shell解析器

#### 5.HISTSIZE

保存历史命令的条数

#### 6.USER

当前登录用户的名字

#### 7.HOME

当前登录用户的主目录

#### 8.PWD

当前工作目录

#### 9.CLASSPATH

java语言库文件搜索的目录，它不是Linux缺省的环境变量，但对java程序员来说非常重要。
<br/>

## 4.设置环境变量

```
变量名='值'
export	变量名

或者
export 变量名='值'

如果环境变量的值中没有空格等特殊的字符，单引号可以不写
```

**采用export配置的环境变量在退出Shell后环境变量将会失效。**

#### 系统环境变量

系统环境变量对全部要用户都生效，设置系统环境有三种方法。需要root用户权限

<br/>

<br/>

#### 1.在/etc/profile文件中设置

用户登录时执行/etc/profile文件中设置系统的环境变量。但是不推荐使用

```
vi /etc/profile

export VAR='hello'
```

```
使用 env  $VAR查看环境变量的值
```

<br/><br/>

####  2.在/etc/profile文件目录中增加环境变量脚本文件,这是linux推荐的方法

```
cd /etc/profile.d

vi var.sh  //新建打开脚本文件var.sh
export VAR='hello'
```

```
退出重新登录后，环境变量生效
env $VAR
```

<br/>

<br/>

####  3.在/etc/bashrc文件中设置环境变量

```
该文件配置的环境变量将会影响全部用户使用的bash shell 。但是不建议使用
```

```
vi /etc/bashrc

export VAR='hello'
```

```
退出重新登录后，环境变量使用
env $VAR	
```

<br/>

<br/>

### 2.用户环境变量

在用户的主目录中，有几个特别的文件，用ls是看不见的，用`ls .bash_*`可以看见。设置用户环境变量有多种方法

1.  **.bash_profile(推荐首选)**

当用户登录时执行，每个用户都可以使用该文件来配置专属自己的环境变量

```
source .bash_profile	//使环境变量生效
```

2.  **.bashrc**

 当用户登录时以及每次打开新的Shell文件时，该文件都将会被读取，不推荐里面配置用户专用的环境变量，因为每开一个Shell，该文件都会被读取一次，效率肯定受影响。
3.  **.bash_logout**

 当每次退出系统（退出bash shell）时执行该文件。
4.  **.bash_history**

保存当前用户使用过的历史命令

环境变量脚本文件的执行顺序

**/etc/profile --> /etc/profile.d --> /etc/bahrc --> 用户的.bash_profile --> 用户的.bashrc**

如果同名的话，后面的环境变量会覆盖前面的环境变量
<br/>

<br/>

## 5.重要的环境变量

### 1.PATH环境 变量

**1.PATH环境变量存放的是目录列表，目录之间用冒号 : 分隔，最后的原点 . 表示当前目录**

```
export PATH=目录1:目录2:目录3:......目录n:.
```

**2.PATH缺省包含了linux系统命令所在的目录（/usr/local/bin:/usr/local/sbin:/usr/sbin），如果不包含这些目录,linux的常用命令也无法执行（要输入绝对路径才能执行）。**

**3.在用户的.bash_profile文件中，会对PATH进行扩充**

<br/>

<br/>

####  2.LANG环境变量

LANG环境变量存放的是linux系统的语言、地区、字符集，它不需要系统管理员手工设置，/etc/profile会调用/etc/profile.d/lang.sh脚本完成对LANG的设置。

<br/>

<br/>

#### 3.CLASSPATH

java语言库文件搜索的目录，他不是linux缺省的环境变量，但对java程序员来说很重要。

CLASSPATH环境变量存放的也是目录列表，目录之间用冒号:分隔,最后的圆点.表示当前目录。
