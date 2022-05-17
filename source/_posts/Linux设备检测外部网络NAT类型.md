---
title: Linux设备检测外部网络NAT类型
date: 2022-05-15 09:53:07
tags:
categories: linux
doc:
---



在异地组网的过程中发现，对称型NAT(Symmetric NAT)对P2P穿透的支持很不好，因此在搭建P2P穿透个人使用的客户端或服务器之前有必要搞清楚自己所在网络环境的NAT类型。
 Windows设备的NAT类型检测工具网上一搜一大把，而Linux的检测工具比较难找，这里提供一种方法和教程。

**工具：pystun**
 pystun是一个STUN客户端工具，用于获取局域网的NAT类型和公网IP。
 A Python STUN client for getting NAT type and external IP (RFC 3489)

**环境：centos8.2（理论上其他Linux发行版本也可以）**
 **注意：必须要用python2.x的环境（推荐2.7），python3.x会报错**

安装：

```
wget https://files.pythonhosted.org/packages/a9/72/0b6a4f8ad71f72ed3b3946a21084a8b3c17de10ddc5fc0dfec43c48fc768/pystun-0.1.0.tar.gz
```

可能有更新的版本，pystun下载地址：https://pypi.python.org/pypi/pystun

```
tar -zxvf pystun-0.1.0.tar.gz
cd pystun-0.1.0/
dnf install python2 -y
python2 setup.py install
```

如果出现下面的情况：

> Traceback (most recent call last): File “setup.py”, line 2, in <module>
>  from setuptools import setup, find_packages
>  ImportError: No module named setuptools

centos：

```
sudo dnf -y install python-setuptools
```

ubuntu：

```
sudo apt-get install python-setuptools
```

就可以解决。

```
pystun 
```

如果用的是python3.x则会出现下面的错误：

> Traceback (most recent call last):
>  File “/usr/local/bin/pystun”, line 33, in 
>  sys.exit(load_entry_point(‘pystun==0.1.0’, ‘console_scripts’, ‘pystun’)())
>  File “/usr/local/lib/python3.6/site-packages/pystun-0.1.0-py3.6.egg/stun/cli.py”, line 55, in main
>  stun_port=options.stun_port
>  File “/usr/local/lib/python3.6/site-packages/pystun-0.1.0-py3.6.egg/stun/**init**.py”, line 253, in get_ip_info
>  stun_host=stun_host, stun_port=stun_port)
>  File “/usr/local/lib/python3.6/site-packages/pystun-0.1.0-py3.6.egg/stun/**init**.py”, line 186, in get_nat_type
>  _initialize()
>  File “/usr/local/lib/python3.6/site-packages/pystun-0.1.0-py3.6.egg/stun/**init**.py”, line 93, in _initialize
>  dictValToAttr.update({items[i][1]: items[i][0]})
>  TypeError: ‘dict_items’ object does not support indexing

运行正常：

> [root@iZbp1fn5lyz904h3d2r8pnZ ~]# pystun
>  NAT Type: Full Cone
>  External IP: 121.196.xxx.xxx
>  External Port: 5xxxx