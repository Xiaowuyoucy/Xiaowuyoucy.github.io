---
title: read函数阻塞和非阻塞
date: 2022-01-10 13:24:06
tags:
categories: linux
doc:
---



通过读普通文件测试得知: read函数在读完文件内容之后, 若再次read,则read函数会立刻返回, 表明read函数读普通文件是非阻塞的.

​	设备文件: `/dev/tty `  标准输入`STDIN_FILENO`
通过读`/dev/tty`终端设备文件, 表明read函数读设备文件是阻塞的.

<br/>

结论: 阻塞和非阻塞不是read函数的属性, 而是文件本身的属性.
socket(套接字)，pipe（管道）这两种文件都是阻塞的.

