---
title: 修改文件权限
date: 2022-01-07 23:34:17
tags:
categories: linux
doc:
---

## 	文字设定法： 

##### `chmod [who] [+|-|=] [mode]`

​	`	who:`
​		    文件所有者： u
​		    文件所属组： g
​		    其他人：        o		     
​		    所有的人：    a
​		    

`+：`添加权限
` -：`减少权限
`=：`覆盖原来的权限

`mode：`
   r：读   w：写   x：执行

```
chmod go-rw 文件名
chmod go+rw 文件名
chmod go=w 文件名
```

## 数字设定法

##### `chmod 权限数字 文件名`

```
root@yc:/home/yxc19980620c/my# ll
drwxr-xr-x  3 yxc19980620c root 12288 1月   7 23:42 ./
drwxr-xr-x 41 yxc19980620c root  4096 1月   7 16:30 ../
-rw-r--r--  1 yxc19980620c root    60 12月 28 03:19 add.c
-rw-r--r--  1 yxc19980620c root  1232 12月 29 01:04 add.o
```

​	`-`：没有权限
​	r：4
​	w：2
​	x：1

`765`
7  --  rwx  --文件所有者&emsp;&emsp;&emsp;&emsp;	其中7代表文件所有者具有读写执行权限

6  --  rw  --文件所属组&emsp;&emsp;&emsp;&emsp;6代表文件所属组具有读写权限

5  --  rx  --其他人&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;5代表其他人具有读和执行权限

```
chmod 765 文件名
chmod -001 文件名    //减去其他人的执行权限
```

