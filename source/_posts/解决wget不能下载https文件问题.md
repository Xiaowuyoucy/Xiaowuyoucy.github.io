---
title: 解决wget不能下载https文件问题
date: 2022-05-15 09:45:22
tags:
categories: linux
doc:
---

使用[wget](https://so.csdn.net/so/search?q=wget&spm=1001.2101.3001.7020)下载https协议的文件时遇到了这个问题，提示：

```
ERROR: cannot verify xx.xx.com's certificate, issued by `/C=BE/O=GlobalSign nv-sa/CN=GlobalSign Organization Validation CA - SHA256 - G2':
  Unable to locally verify the issuer's authority.
ERROR: certificate common name `*.alicdn.com' doesn't match requested host name `xx.xx.com'.
To connect to xx.xx.com insecurely, use `--no-check-certificate'.
无法建立 SSL 连接。
```

错误提示中建议使用“--no-check-certificate”参数，用法是这样的： 

```
wget https://xx.xx.com/xx.exe --no-check-certificate
```

