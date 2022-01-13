---
title: 设置或查看命令别名
date: 2022-01-08 13:01:14
tags:
categories: linux
doc:
---

### 查看 

```
alias 命令名
alias ls
```

### 设置

```
alias 新命令名='命令操作'
alias pag='ps aux | grep'
```

​		需要长久有效需要去设置配置文件：`~/.bashrc`