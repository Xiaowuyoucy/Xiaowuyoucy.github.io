---
title: ssh远程登录到服务器
date: 2022-01-08 01:02:21
tags:
categories: linux
doc:
---

### 安装：

```
sudo apt-get install openssh-server
```

### 查看是否安装ssh

```
sudo aptitude show openssh-server
```

配置文件：`/etc/ssh/ssh_config`

#### 远程登录：

```
ssh 用户名@ip
输入yes或no
输入密码
```

#### 退出登录：

```
logout
```

