---
title: ifconfig查看ip
date: 2022-01-08 00:21:35
tags:
categories: linux
doc:
---

### ifconfig查看ip

```
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.44.135  netmask 255.255.255.0  broadcast 192.168.44.255
        inet6 fe80::7438:27c1:6044:963d  prefixlen 64  scopeid 0x20\<link>
        ether 00:0c:29:f2:c7:2a  txqueuelen 1000  (以太网)
        RX packets 28453  bytes 18295064 (18.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10656  bytes 815687 (815.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10\<host>
        loop  txqueuelen 1000  (本地环回)
        RX packets 945  bytes 84825 (84.8 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 945  bytes 84825 (84.8 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

ens33: 代表设备名      	broadcast 广播地址		inet： ip地址