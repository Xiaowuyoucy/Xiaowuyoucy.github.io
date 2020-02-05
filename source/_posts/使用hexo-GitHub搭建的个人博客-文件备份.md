---
title: 使用hexo+GitHub搭建的个人博客 文件备份
date: 2020-02-05 20:10:54
tags:
categories: githubHexo
doc:
---

# 	使用hexo+GitHub搭建的个人博客 文件备份

### 1.在GitHub中创建 hexo 分支来存储本地 markdown 文件

 在本地文件根目录创建 `.gitignore` 文件，若存在修改为 

```
.DS_Store
*.log
node_modules/
.deploy*/
public/
db.json
```

 在本地文件根目录中初始化 git 

```
git init
```

 创建分支hexo 

```
git checkout -b hexo
```

 提交到仓库，需要注意的事在提交之前要把themes目录下主题中的 `.git` 文件夹重命名或者删除，不然的话 git 会把主题当做子模块来处理。 

```
git add .
git commit -m 'init'
```

 添加远程仓库 

```
git remote add origin git@github.com:MrWangwj/MrWangwj.github.io.git
```

 push 到远程分支 

```
git push origin hexo
```

### 2.在另一台电脑上使用

 首先要克隆下这个项目 

```
git clone git@github.com:MrWangwj/MrWangwj.github.io.git
```

 进入博客目录 

```
cd MrWangwj.github.io.git
```

 切换到博客文件分支 

```
git checkout -b hexo origin/hexo
```

 安装hexo 

```
npm install hexo --save
```

 然后编辑、查看 

```
hexo g    //编译
hexo s    //浏览器查看 localhost:4000
```

 提交 git，若在提交过程中出现 `ERROR Deployer not found: git` 可执行 `npm install hexo-deployer-git --save` 后重新提交。 

```
hexo d
```

 在写了新 markdown 文件后提交 git 

```
git add .
git commit -m '新增博客'
git push origin hexo
```

 到此，我们以后只要写完博客发布后记得 push 一下就能实现备份了。 