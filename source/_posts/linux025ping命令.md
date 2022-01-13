---
title: ping命令
date: 2022-01-08 00:21:39
tags:
categories: linux
doc:
---

### ping 命令

检查网络是否通

`ping ip地址`

```
ping 192.168.1.123
```

<br /><br />

`ping ip地址 -c 次数`

```
ping 192.168.1.123 -c 5 	//ping多少次
```

<br /><br />

`ping ip地址 -i 秒数`

```
ping 192.168.1.123 -i 5		//每隔5秒返回一次结果
```

<br /><br />

`ping 域名`

```
ping www.baidu.com
```

