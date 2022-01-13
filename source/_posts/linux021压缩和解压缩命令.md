---
title: 压缩和解压缩命令
date: 2022-01-08 00:09:55
tags:
categories: linux
doc:
---

## gzip 

压缩出来的文件以.gz结尾
	不保留源文件   不能对目录进行打包压缩

```
gzip 文件名
gzip *.txt
```

​	

解压： 

```
gunzip 文件名
gunzip *.gz
```

<br /><br /><br />




## bzip2

​	后缀名为:`.bz2`

  -k 来保留源文件  不能对目录进行打包压缩
​	

```
bzip2 文件名
bzip2 *.txt
bzip2 -k  *.txt
```

<br /><br /><br />




## tar 

不使用`z`或`j`参数，该命令只能对文件或目录打包
	c--创建---压缩
	x--释放---解压缩
	v--显示提示信息
	f--指定压缩文件的名字
	

z--使用gzip的方式压缩文件
j--使用bzip2的方式压缩文件

压缩：

```
tar -zcvf 生成的压缩包的名字（xxx.tar.gz） 要压缩的文件或目录
tar -jcvf  生成的压缩包的名字（xxx.tar.bz2）要压缩的文件或目录
```

解压：

```
tar -zxvf 压缩包名字 		 //解压到当前目录
tar -jxvf 压缩包名字  		//解压到当前目录	

tar -zxvf 压缩包名字 -C（大写） 指定的目录
tar -zxvf xxx.tar.gz -C ./test
tar -jxvf xxx.tar.gz -C ./test
```



<br /><br /><br />


## rar 

​	压缩：

```
rar a 压缩包的文件名 源文件
rar a bird *.c
```

​	解压：

```
rar x 压缩包名 （解压到当前文件夹）
rar x 压缩包名  解压的路径
rar x bird.rar
rar x bird.rar ./
```

<br /><br />

<br />


## zip

​	参数： 压缩目录要加参数 -r
​	压缩：

```
zip 压缩包的名字 压缩的文件或目录
zip man *.txt
```

​	解压缩：

```
unzip 压缩包的名字
unzip 压缩包的名字 -d 解压目录
```

