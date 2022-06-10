---
title: Shell脚本
date: 2022-06-04 19:01:14
tags:
categories: linux
doc:
---

## shlle脚本介绍

- 什么是shell脚本?
  - 一系列的shell命令的集合, 可以有函数, 条件判断/循环语句, 这样的一个文件叫做shell脚本

- 基本格式?

  - 命名格式
    - 一般使用.sh为后缀命名文件 (这是一个约定, 不是必须的)
  - 书写格式

  

  cat /etc/shells  	//查看解析器
  echo $SHELL 	//查看当前默认的解析器

```
# - 注释

# 第一行的内容
#! /bin/bash - 解析当前脚本文件使用的命令解析器
#! /bin/sh
bourne shell ->sh -> unix
bourne again shell -> bash

# 第二行开始
shell命令 1
shell命令 2
shell命令 3
......
```

### 执行脚本

脚本创建出来之后是一个普通文件 -> 没有执行权限

```
chmod u+x xxx.sh
./xxx.sh -> 最常用的方式
sh xxx.sh
```





### shell中的变量

1. 如何定义变量

```
# c
int a;
int number = 9;
# shell - 变量没有数据类型, 新变量定义必须初始化
value=123
value1="123"
value1="hello,world"
赋值的时候=前后不能有空格
```



2. 位置变量 - 执行shell脚本时候, 传递到内部的参数

```
$0 - 脚本文件的名字
$1 - 第一个参数
$2 - 第二个参数
$3 - 第三参数
...
$n - 第n个参数
```



```
#!/bin/bash
echo $0
echo $1
echo $2
echo $3
echo $4
echo $5
echo $6
# 执行脚本
./test.sh 1 2 3 4 a b c d e f
# 输出结果
./test.sh
1
2
3
4
a
b
```



3. 特殊变量

```
$#: 表示参数的个数
$@: 表示全部的参数

$$ : 当前进程的PID
$?: 程序执行完成之后的返回值

4. 取普通变量的值

第一种方式: $变量名
$PATH
$value

第二种方式: ${变量名}
${PATH}
${value}

5. 取命令执行之后的得到的值
第一种方式
value=$(shell命令)

第二种方式:
value=`shell命令`
```

##### `使用""和''的区别:对于""中的变量会直接展开, 而对于''当做普通字符串对待`

```
例如: echo "$HOME"  和echo '$HOME'
```



### 条件判断语法 - if

```
# 条件判断语句和[ 判断语句 ]左右必须要有空格间隔
if[ 判断语句 ];then
处理语句
处理语句
fi
# 或者
if[ 判断语句 ]
then
处理语句
处理语句
fi
```



```
if [ 判断语句 ];then
处理语句
处理语句
elif [ 判断语句 ];then
处理语句
处理语句
else
处理语句
处理语句
fi
```

1. 文件状态测试

```
-b filename 		当filename 存在并且是块文件时返回真(返回0)
-c filename 		当filename 存在并且是字符文件时返回真
-d pathname 		当pathname 存在并且是一个目录时返回真
-e pathname 		当由pathname 指定的文件或目录存在时返回真
-f filename 		当filename 存在并且是正规(普通)文件时返回真
-g pathname 		当由pathname 指定的文件或目录存在并且设置了SGID 位时返回真
-h/-L filename		当filename 存在并且是符号链接文件时返回真 (或 filename)					
-k pathname 		当由pathname 指定的文件或目录存在并且设置了"粘滞"位时返回真
-p filename 		当filename 存在并且是命名管道时返回真
-r pathname 		当由pathname 指定的文件或目录存在并且可读时返回真
-s filename 		当filename 存在并且文件大小大于0 时返回真
-S filename 		当filename 存在并且是socket 时返回真
-t fd 				当fd 是与终端设备相关联的文件描述符时返回真
-u pathname 		当由pathname 指定的文件或目录存在并且设置了SUID 位时返回真
-w pathname 		当由pathname 指定的文件或目录存在并且可写时返回真
-x pathname 		当由pathname 指定的文件或目录存在并且可执行时返回真
-O pathname
					当由pathname 存在并且被当前进程的有效用户id 的用户拥有时返回真(字母O 大写)
-G pathname 		当由pathname 存在并且属于当前进程的有效用户id 的用户的用户组时返回真
file1 -nt file2 	file1 比file2 新时返回真
file1 -ot file2 	file1 比file2 旧时返回真
f1 -ef f2 			files f1 and f2 are hard links to the same file
```

2. 常见字符串测试

```
-z string 		字符串string 为空串(长度为0)时返回真
-n string 		字符串string 为非空串时返回真
str1 = str2 	字符串str1 和字符串str2 相等时返回真
str1 == str2 	同 =
str1 != str2 	字符串str1 和字符串str2 不相等时返回真
str1 < str2 	按字典顺序排序，字符串str1 在字符串str2 之前
str1 > str2 	按字典顺序排序，字符串str1 在字符串str2 之后
```

3. 常见数值测试

```
nt1 -eq int2 		如果int1 等于int2，则返回真
int1 -ne int2 		如果int1 不等于int2，则返回真
int1 -lt int2 		如果int1 小于int2，则返回真
int1 -le int2 		如果int1 小于等于int2，则返回真
int1 -gt int2 		如果int1 大于int2，则返回真
int1 -ge int2 		如果int1 大于等于int2，则返回真
```

4. 测试时使用的逻辑操作符

```
-a 		逻辑与，操作符两边均为真，结果为真，否则为假。
-o 		逻辑或，操作符两边一边为真，结果为真，否则为假。
! 		逻辑否，条件为假，结果为真。
```





### 循环控制

```
循环:
    for语句和while
    例1:
    for var in apple pear banana
    do
    echo $var
    done
    
    例2:打印当前所有的文件
    for file in `ls`
    do
    echo $file
    done
    
    例3:求1-100的和
    sum=0
    for i in {1..100}
    do
    sum=$[$sum+$i]
    done
    echo "sum==[$sum]"
    如果一个文件名字为: file.xxx, 想去掉~~得到file可以: basename file.xxx ".xxx"
    
    使用这种方法可以获得文件名字去掉扩展名后的名字: 如: basename test.sh .sh
    案例: 某个目录下有a.bak b.bak c.bak d.bak, 要求把扩展名去掉, 变成a b c d
    mv a.bak $(basename a.bak .bak)
    
    while的用法:
    案例: 求1-10的和
    sum=0
    i=0
    while [ $i -le 10 ]
    do
    sum=$[$sum+$i]
    i=$[$i+1]
    done
    echo "sum==[$sum]"
```

整数的运算要用$[]括起来

### awk介绍

awk 可以将文件拆分成功若干行, 根据指定的分隔符, 再将每一行拆分成若干列, 默认按照空格或tab进行拆分

1. 基本语法格式

```
awk 参数 '条件{处理动作}' 操作的文件或数据
awk 参数 '/正则表达式{处理动作}' 操作的文件或数据
```

2. 指定分隔符的参数: -F
   - -F分隔符
   - 如果不指定分隔符, 默认是按照空格或者tab进行拆分
3. 如何使用变量取出某一列?
   - $0 - 当前行
   - $1 - 拆分的第一列
   - $2 - 拆分的第二列
   - $3 - 拆分的第三列
   - ...
   - 打印某一列的值
     - print $n

```
awk -F ':' '{print $0}' /etc/passwd
代表/etc/passwd文件中的内容以:分割,然后打印当前行数据
```



### 使用shell脚本结束某个进程

```
#! /bin/bash
PID=`ps -ef|grep xxx|grep -v grep|awk '{print $2}'`
#grep -v grep 代表去除最后的grep

#如果PID不为空 则结束进程
if [-n $PID]
then
	kill -9 $PID
else
	echo "没有可结束的进程"
fi
```

















