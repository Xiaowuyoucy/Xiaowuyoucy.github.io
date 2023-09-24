---
title: 安装与配置Linux操作系统
date: 2023-04-10 22:29:52
tags:
categories: RHEL8
doc:
---

Linux一般由3个部分组成：内核(kernel)、命令解析层（Shell或其他操作环境）、实用工具。



### 重置root管理员密码

（1）先确认是RHEL8

```
cat /etc/redhat-release
```

（2）在终端输入reboot，重启之后按E进入内核编辑模式



（3） 在linux参数行的最后面追加 “rd.break” 然后按下CTRL+X 运行修改过的内核程序



（4）进入紧急模式。依次输入以下命令

```
mount -o remount,rw /sysroot
chroot /sysroot
passwd   这一步按下回车后要输入新密码和确认新密码,输入过程不显示
touch /.autorelabel
exit
reboot
```



### yum软件仓库

yum软件仓库是为了解决安装软件的时候,如果需要大量的依赖,安装起来是非常痛苦的,所以yum的出现是为了降低软件安装的难度。



RHEL 先将发布的软件存放到yum服务器内，然后分析软件的依赖属性，将分析出来的软件信息生成清单列表

容器：软件清单列表数据和软件所在的位置称为容器

Linux用户安装程序---> 先yum服务器中的容器发起下载xxxx软件清单的请求-------客户机得到清单列表后会和本机的RPM数据库做比较,比较后把不存在的依赖一步下载过来就可以了.



 