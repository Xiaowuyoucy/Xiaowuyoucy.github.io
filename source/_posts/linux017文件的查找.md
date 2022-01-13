---
title: 文件的查找
date: 2022-01-07 23:48:39
tags:
categories: linux
doc:
---

## 	按文件属性查找：

### 文件名：	

```
find 查找的目录 -name “文件的名字”
```

通配符：*代表所有字符   ？ 代表一个字符

<br/><br/>

### 文件大小：

```
 find 查找的目录  / -size +10k
```

·			 find 查找的目录 / -size -10k
·			 find 查找的目录 / -size -10M
			 find 查找的目录 / -size +10k -size -10M  		//表示大于10KB小于10MB的文件
			+代表大于       - 代表小于   
			数字后面的字母区分大小写
			<br/><br/><br/>

### 文件类型：

find 查找的目录 -type 文件类型

```
find  / -type s
find  / -type s
```

<br/><br/><br/>


## 按文件内容查找

```
grep -r “查找内容” 查找路径
```

​	-r  代表递归查找

