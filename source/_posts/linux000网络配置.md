---
title: 网络配置
date: 2022-02-08 18:24:04
tags:
categories: linux
doc:
---

# Ubuntu18.04的网络配置（静态IP和动态IP）

### 查看Ubuntu系统的版本号命令

`cat /etc/issue `或者 `lsb_release -a`

### 切换root命令

`sudo -i` 或者 `sudo -s`

**提示：以下操作均在root用户下进行，如在普通用户，请自行加上sudo！**

### 说明

Ubuntu从17.10开始，已放弃在`/etc/network/interfaces`里配置IP地址，即使配置也不会生效，而是改成netplan方式，配置写在`/etc/netplan/01-netcfg.yaml`或者类似名称的yaml文件里，如下：

VMware14里安装的Ubuntu18.04.1 Desktop版本下的配置文件名：

![img](/images/javawz/1404518-20200614223934337-1576886527.png)

VMware14里安装的Ubuntu18.04.4 Server版本下的配置文件名：

![img](/images/javawz/1404518-20200614224306528-1449186048.png)

阿里云Ubuntu18.04.4 Server版本下的配置文件名：

![img](/images/javawz/1404518-20200614224537679-1678462440.png)

### 下面以VMware14里安装的Ubuntu 18.04.4 Server版本为例（VMware网络连接选择的桥接模式）

#### 一、配置静态IP地址

打开配置文件：`vim /etc/netplan/50-cloud-init.yaml`，写入以下配置内容：

```
network:
    ethernets:
        ens33:                  # 配置的网卡名称
            dhcp4: no           # 关闭dhcp4
            dhcp6: no           # 关闭dhcp6
            addresses: [192.168.0.120/24]       # 设置本机IP地址及掩码
            gateway4: 192.168.0.1               # 设置网关
            nameservers:
                    addresses: [114.114.114.114, 8.8.8.8]       # 设置DNS
    version: 2
```

截图

![img](/images/javawz/1404518-20200614231300863-237073454.png)

配置完成后，保存并退出，执行` netplan apply `命令可以让配置直接生效

以前的重启网络服务命令 `/etc/init.d/networking restart` 或者 `service networking restar` 都是无法使用的（做测试时发现18.04.1的Desktop版本还是可以使用的，但/etc/netplan/下的yaml配置文件并不会生效）。

#### 验证是否配置成功

`ifconfig -a`

![img](/images/javawz/1404518-20200614222201955-990276377.png)

#### 验证是否能ping通外网

`ping -c 4 baidu.com`

![img](/images/javawz/1404518-20200614222251773-819407962.png)

#### 二、配置动态IP地址

打开配置文件：`vim /etc/netplan/50-cloud-init.yaml`，写入以下配置内容（其实只需要开启dhcp就可以）：

![img](/images/javawz/1404518-20200614230042002-1549179355.png)

```
network: 
   ethernets: 
       ens33: 			#配置网卡名称
           dhcp4: true		#开启dhcp4
           dhcp4: true		#开启dhcp4
   version: 2
```

保存并退出，执行 `netplan apply `命令让配置生效，用上述方法验证是否配置成功！

 

这里顺便也记录下Ubuntu 18.04.1 Desktop版本的配置，和18.04.4 Server版本略有区别（VMware网络连接选择的也是桥接模式）

`vim /etc/netplan/01-network-manager-all.yaml`

```
network:
  version: 2
  # renderer: NetworkManager
  ethernets:
          ens33:
                  dhcp4: no
                  dhcp6: no
                  addresses: [192.168.0.130/24]
                  gateway4: 192.168.0.1
                  nameservers:
                          addresses: [114.114.114.144, 8.8.8.8]
```

截图

![img](/images/javawz/1404518-20200615081736202-1829516241.png)

### 这里有几点需要注意：

1、Ubuntu 18.04.1 Desktop版本配置的时候需要将renderer: NetworkManager一行注释掉，否则netplan命令无法生效；

2、配置信息要严格按照yaml语言的语法格式，每个配置项使用空格缩进表示层级关系；**缩进不允许使用tab，只允许空格；缩进的空格数不重要，只要相同层级的元素左对齐即可，否则netplan命令会报错；**

3、对应配置项后跟着冒号，之后要接个空格，否则netplan命令也会报错。

## 扩展

重新启停以太网卡命令：

```
ifconfig ens33 down		//关闭	

ifconfig ens33 up		//开启
```

