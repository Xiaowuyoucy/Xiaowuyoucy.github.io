---
title: 查看文件内容
date: 2022-01-07 23:12:56
tags:
categories: linux
doc:
---

## gedit 

gedit 文件名

图形界面的编辑器

## cat 

cat 文件名
cat 文件1 文件2 ... 文件n   拼接查看文件
cat 文件1 | more 

## more 

more 文件名 
	回车  一行
	空格  一页
`	只能往后看`
	q 退出	
	ctrl + c 退出

## less 

less 文件名
	回车  一行
	空格  一页
	`可以往前后看`
	q 退出	
	ctrl + c 退出
	ctrl + p 向前一行
	ctrl + n 向后一行
	ctrl +b 向前一页
	ctrl + f 向后一下

## head

用来显示前面几行

head 文件名      显示文件前十行
head -5 文件名   显示文件前五行

## tail

用来显示最后几行

tail 文件名	 显示文件后十行
tail -5 文件名	 显示文件后五行