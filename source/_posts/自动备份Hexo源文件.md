---
title: 自动备份Hexo源文件
date: 2020-02-05 18:16:47
tags:
categories: githubHexo
doc:
---


# 自动备份Hexo源文件

### 前言

配置一个<span style="color:red">`Hexo`</span>博客往往需要作者倾注大量心血，而如果哪天电脑坏了或者换电脑导致<span style="color:red">`Hexo`</span>源文件丢失的话就是一件比较杯具的事。因此本文给出了一种自动备份<span style="color:red">`Hexo`</span>源文件到<span style="color:red">`Github`</span>的方法，能够在执行<span style="color:red">`hexo deploy`</span>命令后自动执行<span style="color:red">`Git`</span>命令以推送<span style="color:red">`Hexo`</span>源文件到<span style="color:red">`Github`</span>仓库。

#### 原理

<span style="color:red">`NodeJS`</span>的事件监听机制能够监听<span style="color:red">`Hexo`</span>的事件。通过查询[Hexo文档](https://hexo.io/zh-cn/api/events.html)，找到了<span style="color:red">`Hexo`</span>的主要事件，见下表：

| 事件名           | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| `deployBefore`   | 在部署完成前发布。                                           |
| `deployAfter`    | 在部署成功后发布。                                           |
| `exit`           | 在`Hexo` 结束前发布。                                        |
| `generateBefore` | 在静态文件生成前发布。                                       |
| `generateAfter`  | 在静态文件生成后发布。                                       |
| `new`            | 在文章文件建立后发布。该事件返回文章参数。                   |
| `processBefore`  | 在处理原始文件前发布。此事件会返回一个地址，代表 Box（Box）的根目录。 |
| `processAfter`   | 在原始文件处理后发布。此事件会返回一个地址，代表 Box（Box）的根目录。 |
| `ready`          | 在初始化完成后发布。                                         |

通过查询上表发现，我们可以通过监听<span style="color:red">`Hexo`</span>的<span style="color:red">`deployAfter`</span>事件，待部署成功后自动运行<span style="color:red">`Git`</span>备份命令，从而达到自动备份的目的。

### 实现

#### 将<span style="color:red">`Hexo`</span>目录加入<span style="color:red">`Git`</span>仓库

首先需要在<span style="color:red">`Github`</span>创建一个新的<span style="color:red">`repository`</span>,名字与本地<span style="color:red">`Hexo`</span>文件夹同名即可。然后进入本地<span style="color:red">`Hexo`</span>文件夹，按顺序执行以下命令：

```
git init
git remote add origin git@github.com:yourname/hexo.git
git pull origin master
```

再每次执行<span style="color:red">`hexo generate`</span>命令时，<span style="color:red">`public/、.deploy、\*.log `</span>文件夹会重写更新，因此需要在<span style="color:red">`.gitignore`</span>文件（如果没有，手动创建一个）中写入<span style="color:red">` public/、`</span><span style="color:red">`.deploy、`</span><span style="color:red">`\*.log `</span>以忽略这几个目录，加快备份的速度。

然后再顺序执行以下命令，将文件推送到<span style="color:red">`Github`</span>：

```
git add .
git commit -m "备份hexo源码文件"
git push origin master
```

这三行命令便是手动推送本地文件到<span style="color:red">`Github`</span>的命令,我们接下来要做的是让<span style="color:red">`NodeJS`</span>监听到<span style="color:red">`Hexo`</span>的<span style="color:red">`deployAfter`</span>事件后自动执行上述命令。

###  安装<span style="color:red">`shelljs`</span>模块

键入以下命令即可

```
npm install --save shelljs
```

##### 加入自动执行脚本

在<span style="color:red">`Hexo`</span>根目录的<span style="color:red">`scripts`</span>文件夹（没有就自己创建一个）下新建一个<span style="color:red">`js`</span>文件，文件名随意。然后加入以下代码：

```javascript
require('shelljs/global');

try {
	hexo.on('deployAfter', function() {//当deploy完成后执行备份
		run();
	});
} catch (e) {
	console.log("产生了一个错误<(￣3￣)> !，错误详情为：" + e.toString());
}

function run() {
	if (!which('git')) {
		echo('Sorry, this script requires git');
		exit(1);
	} else {
		echo("======================Auto Backup Begin===========================");
		cd('path to hexo');    //此处修改为Hexo根目录路径
		if (exec('git add --all').code !== 0) {
			echo('Error: Git add failed');
			exit(1);

		}
		if (exec('git commit -am "Form auto backup script\'s commit"').code !== 0) {
			echo('Error: Git commit failed');
			exit(1);

		}
		if (exec('git push origin master').code !== 0) {
			echo('Error: Git push failed');
			exit(1);

		}
		echo("==================Auto Backup Complete============================")
	}
}

```

注意上述代码中第29行需要修改为自己<span style="color:red">`Hexo`</span>文件夹的绝对路径。

#### 效果

如果脚本运行成功，会得到类似以下的效果：

```

======================Auto Backup Begin===========================
cd: no such file or directory: C:/Users/17810/OneDrive/hexo
warning: LF will be replaced by CRLF in source/_posts/自动备份Hexo源文件.md.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in source/_posts/自动备份Hexo源文件.md.
The file will have its original line endings in your working directory.
[master a6cb4c7] Form auto backup script's commit
 1 file changed, 153 insertions(+)
warning: LF will be replaced by CRLF in source/_posts/自动备份Hexo源文件.md.
The file will have its original line endings in your working directory.
To git@github.com:JacobZjw/hexo.git
   0beaf4c..a6cb4c7  master -> master
==================Auto Backup Complete============================
```

### 懒癌患者的小福利

在<span style="color:red">`hexo`</span>中，我们常常使用<span style="color:red">`hexo new`</span>命令来新建文章。然鹅，当我们的文章较多时，我们往往需要在成堆的文章中找到刚刚生成的文件，然后使用<span style="color:red">`Markdown`</span>编辑器打开。作为一名懒癌患者怎么可能让自己如此受累。

于是，我找到了<span style="color:red">`Hexo`</span>作者给出的解决办法

和上面类似，在<span style="color:red">`scripts`</span>文件夹中创建一个<span style="color:red">`js`</span>文件，添加以下代码：

```javascript
var spawn = require('child_process').exec;

// Hexo 2.x 用户复制这段
hexo.on('new', function(path){
  spawn('start  "markdown编辑器绝对路径.exe" ' + path);
});

// Hexo 3 用户复制这段
hexo.on('new', function(data){
  spawn('start  "markdown编辑器绝对路径.exe" ' + data.path);
});

```

 保存并退出脚本之后，在命令行中键入： 

```
hexo new "auto open editor test"
```

 是不是就顺利的自动打开了自动生成的md文件啦~ 