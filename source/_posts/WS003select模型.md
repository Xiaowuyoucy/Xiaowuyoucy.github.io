---
title:  select模型
date: 2021-06-21 22:48:18
tags:
categories: Windows网络编程
doc:
---

# select模型

## 特点

### 1、解决基本c/s模型中，accept rcev 傻等的问题

- 傻等阻塞
- 执行阻塞

	- send recv accept 
	- 在执行的复制黏贴的过程中都是阻塞的
	- 所以注意，select没模型是解决傻等的问题的，不解决这几个函数本身的阻塞问题，这几个函数本身阻塞的问题，我们接下来的模型会慢慢深入讲解，有五种模型呢~

### 2、实现多个客户端链接，与多个客户端分别通信

### 3、用于服务器，客户端就不用这个了，因为只有一个socket

- 客户端recv等时候，也不能send啊？
- 只需要单独创建一根线程，recv放线程里，就完事儿~

## 服务端

### 网络头文件 网络库 

- 最底层的网络函数，大家用QT MFC wpf，或者百度下载的很多其他的封装好的网络库，都是对咱们讲的这些最本质的网络函数的二次封装，咱们讲的是通用的，讲完这个大家也可以自己封装函数库给别人用了。
- #include <WinSock2.h>
#pragma comment(lib,"ws2_32.lib")

	- 函数库

		- winsock2.h

			- windows socket 第2版

				- 第一版是 winsock.h
				- 查看具体区别

			- 目前网络库有哪些版本？

				- 1.0
1.1
2.0
2.1
2.2

					- 演示MSDN

						- 查这个函数的详细信息

							- WSAStartup

			- 我们的开发环境支持哪个版本？

				- 打开头文件，看咱们当前编译器环境支持的最高版本

		- ws2_32.lib

			- windows socket 第2版  32位

				- 不管是64编译环境还是32编译环境，都是用这个，并没有ws2_64.dll

		- wsock32.lib

			- winsock.h 第一版网络库 对应的库文件

		- 这里不区分大小写

### 打开网络库

- int WSAStartup(
  WORD      wVersionRequired,
  LPWSADATA lpWSAData
);

	- 功能

		- 打开网络库/启动网络库，启动了这个库，这个库里的函数/功能才能使用
		- w windows
s  socket
a Asynchronous  异步
startup   启动

			- 同步与异步

				- 同步

					- 阻塞/卡死状态

				- 异步

					- 多个工作同时进行

	- 参数1

		- 我们要使用的库的版本
		- 类型是 WORD

			- 转定义：unsigned  short

		- WORD wVersionRequired = MAKEWORD(2,2);

			- MAKEWORD(主版本,副版本)， 1.0 2.2  
			- wVersionRequired

				- 数据高位/高地址是副版本
				- 数据低位/低地址是主版本
				- 用调试+计算器演示数据原理

		- 位运算，内存操作，小端存储

	- 参数2

		- LPWSADATA lpWSAData

			- 系统通过这个参数给我们一些配置信息

				- 注意

					- 当看到参数有 LP P前缀的时候，是说我们这里要传对应类型变量的地址，这是win32API 的规范 或者叫规则

			- 看下有哪些信息

				- struct WSAData {
  WORD           wVersion;
  WORD           wHighVersion;
  unsigned short iMaxSockets;
  unsigned short iMaxUdpDg;
  char           *lpVendorInfo;
  char           szDescription[WSADESCRIPTION_LEN + 1];
  char           szSystemStatus[WSASYS_STATUS_LEN + 1];
} 

					- wVersion

						- 我们要使用的版本

					- wHighVersion

						- 系统能提供给我们最高的版本

					- iMaxSockets

						- 返回可用的socket的数量，2版本之后就没用了

					- iMaxUdpDg

						- UDP数据报信息的大小，2版本之后就没用了  

					- lpVendorInfo

						- 供应商特定的信息，2版本之后就没用了  

					- szDescription
szSystemStatus

						- 当前库的描述信息，2.0是第二版的意思

			- 当输入的版本不存在

				- 输入1.3， 2.3

					- 有主版本，没有副版本

						- 得到该主版本的最大副版本 1.1 2.2并使用

				- 输入3.1  3.3

					- 超过最大版本号

						- 使用系统能提供的最大的版本 2.2

				- 输入 0.0 0.1 0.3

					- 主版本是0

						- 网络库打开失败，不支持请求的套接字版本

	- 返回值

		- 返回0为执行正确
		- 失败

			- 这些宏的本质
			- WSASYSNOTREADY   10091       底层网络子系统尚未准备好进行网络通信。                                                  系统配置问题，重启下电脑，检查ws2_32库是否存在，或者是否在环境配置目录下
WSAVERNOTSUPPORTED 10092 此特定Windows套接字实现不提供所请求的Windows套接字支持的版本。      要使用的版本不支持
WSAEPROCLIM        10067         已达到对Windows套接字实现支持的任务数量的限制。                                 Windows Sockets实现可能限制同时使用它的应用程序的数量
WSAEINPROGRESS 10036          正在阻止Windows Sockets 1.1操作。                                                          当前函数运行期间，由于某些原因造成阻塞，会返回在这个错误码，其他操作均禁止
WSAEFAULT            10014          lpWSAData参数不是有效指针。                                                                 参数写错了

### 校验版本

- 2 != HIBYTE(wsaData.wVersion) || 2 != LOBYTE(wsaData.wVersion)

	- HIBYTE是高位 副版本
	- LOBYTE是地位 主版本
	- 逻辑

		- 只要有一个不是2，说明系统不支持我们要的2.2版本

- 关闭库
并结束函数，可以给出相应提示

	- WSACleanup();
	- return 0；

### 创建SOCKET

- SOCKET  socket(
  int af,
  int type,
  int protocol
);

	- 作用

		- 创建一个SOCKET

	- SOCKET介绍

		- 什么是socket

			- 将底层复杂的协议体系，执行流程，进行了封装，封装完的结果，就是一个SOCKET了，
			- 也就是说，SOCKET是我们调用协议进行通信的 操作接口

		- 意义

			- 将复杂的协议过程与我们编程人员分开，我们直接操作一个简单SOCKET就行了，对于底层的协议 过程细节，完全不用知道，这就大大方便了我们。

				- 网络编程难就难在协议本身的复杂性，简单就简单在我们编程层面完全不用考虑哪些

		- 本质

			- 就是一种数据类型，转定义看下类型

				- 就是一个整数

					- uint

				- 但是这个数是唯一的

					- 标识着我当前的应用程序，协议特点等信息
					- ID，门牌号

		- 应用

			- 我们网络通信的函数，全部都要使用SOCKET

				- 演示

			- 逻辑

				- 每个客户端有一个SOCKET，服务器有一个SOCKET，通信时候，就需要这个SOCKET做参数，给谁通信，就要传递谁的SOCKET

		- 所以

			- 网络编程，理论层面SOCKET是网络封装的精华，代码层面就是不停的使用SOCKET这个变量，所以又叫SOCKET编程

	- 参数1

		- 地址的类型

			- 比如大家联系我

				- 手机

					- 15512345678

				- 固定电话

					- 7881234

				- ＱＱ

					- 40916626

				- 微信

					- c3_xin666

				- 找上门

					- 内蒙古 xxxxxxx

				- ....

			- AF_INET  2

				- ipv4

					- Internet协议版本4（IPv4）地址系列。

						- 192.168.1.103

							- 0.0.0.0  ~  255.255.255.255
							- 点分十进制表示法

					- 4字节 32位的地址

						- 个数快不够

							- 就是无符号int类型的范围  0 ~ 4294967295

			- AF_INET6  23

				- ipv6

					- Internet协议版本6（IPv6）地址系列。

						- 2001:0:3238:DFE1:63::FEFB

					- 16字节  128位的地址

						- 这个地球每寸一个IP

			- AF_BTH  32

				- 蓝牙地址系列。
如果计算机安装了蓝牙适配器和驱动程序，则Windows XP SP2或更高版本支持此地址系列。

					- 6B:2D:BC:A9:8C:12

			- AF_IRDA  26

				- 红外数据协会（IrDA）地址系列。
仅当计算机安装了红外端口和驱动程序时，才支持此地址系列。

			- 通信地址不仅仅只有IP地址

	- 参数2

		- 套接字类型

			- SOCK_STREAM    1

				- 一种套接字类型，提供带有OOB数据传输机制的顺序，可靠，双向，基于连接的字节流。 此套接字类型使用传输控制协议（TCP）作为Internet地址系列（AF_INET或AF_INET6）。

			- SOCK_DGRAM    2

				- 一种支持数据报的套接字类型，它是固定（通常很小）最大长度的无连接，不可靠的缓冲区。 此套接字类型使用用户数据报协议（UDP）作为Internet地址系列（AF_INET或AF_INET6）。

			- SOCK_RAW   3

				- 一种套接字类型，提供允许应用程序操作下一个上层协议头的原始套接字。 要操作IPv4标头，必须在套接字上设置IP_HDRINCL套接字选项。 要操作IPv6标头，必须在套接字上设置IPV6_HDRINCL套接字选项。

			- SOCK_RDM   4

				- 一种套接字类型，提供可靠的消息数据报。 这种类型的一个示例是Windows中的实用通用多播（PGM）多播协议实现，通常称为可靠多播节目。
仅在安装了可靠多播协议时才支持此类型值。

			- SOCK_SEQPACKET   5

				- 一种套接字类型，提供基于数据报的伪流数据包。

	- 参数3

		- 协议的类型

			- IPPROTO_TCP

				- 传输控制协议（TCP）。 当af参数为AF_INET或AF_INET6且类型参数为SOCK_STREAM时，这是一个可能的值。

			- IPPROTO_UDP

				- 用户数据报协议（UDP）。 当af参数为AF_INET或AF_INET6且类型参数为SOCK_DGRAM时，这是一个可能的值。

			- IPPROTO_ICMP

				- Internet控制消息协议（ICMP）。 当af参数为AF_UNSPEC，AF_INET或AF_INET6且类型参数为SOCK_RAW或未指定时，这是一个可能的值。

			- IPPROTO_IGMP

				- Internet组管理协议（IGMP）。 当af参数为AF_UNSPEC，AF_INET或AF_INET6且类型参数为SOCK_RAW或未指定时，这是一个可能的值。

			- IPPROTO_RM

				- 用于可靠多播的PGM协议。 当af参数为AF_INET且类型参数为SOCK_RDM时，这是一个可能的值。 在针对Windows Vista及更高版本发布的Windows SDK上，此协议也称为IPPROTO_PGM。
仅在安装了可靠多播协议时才支持此协议值。

			- 整理下

				- 通过参数3得到一个事儿，参数1 2 3三者是配套的，是一套参数，不是随便填的，即使用不同的协议，那要添加对应的那套参数。
				- 想要使用一个协议，咱们设备得支持才行，比如红外
				- 参数3中，有个可能这个词，所以说一般，参数3可以填写0，系统会自动帮我们选择协议类型

	- 返回值

		- 成功返回可用的socket

			- 不用了就一定要销毁套接字

				- closesocket(socketListen);

		- 失败返回INVALID_SOCKET

			- 关闭网络库

				- WSACleanup();

			- 可用WSAGetLasterror()返回错误码

### 绑定地址与端口

- int bind(
  SOCKET              s,
  const sockaddr *addr,
  int                 namelen
);

	- 作用

		- 给我们的socket绑定端口号与具体地址

			- 地址

				- 找到咱们的电脑

					- 只有一个

			- 端口号

				- 找到我们机器上对应的软件，比如QQ，浏览器等等，都对应着自己的端口号

					- 多个

				- 每一种通信的端口号是唯一的
				- 同一个软件可能占用多个端口号

	- 参数1

		- 上一个函数创建了socket，绑定了协议信息（地址类型，套接字类型，协议类型），咱们bind函数就是绑定实质的地址，端口号

	- 参数2

		- 结构体

			- 地址类型
			- 装IP地址
			- 端口号

		- 结构体类型

			- sockaddr
			- 该参数使用方法

				- SOCKADDR_IN sockAddress;
sockAddress.sin_family = AF_INET;
sockAddress.sin_addr.s_addr = inet_addr("127.0.0.1");
sockAddress.sin_port = 12345;
(sockaddr*)&sockAddress强转添加到参数2上

					- 成员1

						- 跟socket函数参数1是一样的

					- 成员2

						- IP地址

							- 192.168.xxx.xxx

								- 可以在控制台输入指令  ipconfig 就能看到了
								- 或者在网络设置中，能找到这个地址
								- 我就不演示了，我不想让你们发现我

							- 127.0.0.1

								- 回送地址  本地回环地址  本地网络测试

					- 参数3

						- 端口号

							- 本质

								- 就是一个整数

									- 0~65535

								- IP是我们机器的地址，端口就是我们具体的软件的通信口，一个软件可能会占用多个接口，比如一个软件可以聊天，可以下载，可以看视频..那么这些不同的通信内容，往往会有各自的协议，各自的端口。

									- IP是公司地址，端口就是各个部门的地址了

							- 填写哪个值呢？

								- 理论上只要这个范围0~65535都可以

									- 实际

										- 介于0～1023，为系统保留占用端口号

											- 21端口分配给FTP(文件传输协议)服务
25端口分配给SMTP（简单邮件传输协议）服务
80端口分配给HTTP服务

									- 所以

										- 我们不能写这个范围的

											- 我们的范围就是1024~65535

												- 稍微大点儿，1万多

											- 但是注意一点，端口号是唯一的，比如1234已经被别的软件占用了，那么你再使用1234这个端口号，那么就会绑定失败，因为已经呗占用了

												- 给大家演示下

											- 那大家如何查看自己要用的端口号有没有被占用呢？

												- 打开运行cmd输入netstat -ano

													- 查看被使用的所有端口

												- netstat -aon|findstr "12345"

													- 检查我们要使用的端口号是否被使用了
													- 使用了就会显示使用的程序，未被使用就啥都不显示

	- 参数3

		- 参数2的类型大小

			- sizeof(参数2)

	- 返回值

		- 成功返回0
		- 失败返回SOCKET_ERROR

			- 具体错误码通过int WSAGetLastError(void);获得 
			- closesocket(socketListen);
WSACleanup();

### 开始监听

- int WSAAPI listen(
  SOCKET s,
  int    backlog
);

	- 作用

		- 将套接字置于正在侦听传入连接的状态。

	- 参数1

		- 服务器端的socket，也就是socket函数创建的

	- 参数2

		- 挂起连接队列的最大长度。

			- 就是说，比如有100个用户链接请求，但是系统一次只能处理20个，那么剩下的80个不能不理人家，所以系统就创建个队列记录这些暂时不能处理，一会儿处理的链接请求，依先后顺序处理，那这个队列到底多大？就是这个参数设置，比如2，那么就允许两个新链接排队的。这个肯定不能无限大，那内存不够了。
			- 我们可以手动设置这个参数，但是别大了。可能2~10多，~20多。
			- 我们一般填写这个参数

				- SOMAXCONN

					- 作用是让系统自动选择最合适的个数
					- 不同的系统环境不一样，所以这个合适的数也都不一样

	- WSAAPI

		- 调用约定

			- 这个我们可以忽略，这是给系统看的，跟咱们没关
			- 决定三

				- 函数名字的编译方式
				- 参数的入栈顺序
				- 函数的调用时间

	- 返回值

		- 成功

			- 返回0

		- 失败

			- SOCKET_ERROR
			- 具体错误码

				-  WSAGetLastError()
				- 释放

					- closesocket(socketListen);
					- WSACleanup();

### select

- 逻辑

	- 1、每个客户端都有socket，服务器也有自己的socket，将所有的socket装进一个数据结构里，即数组

		- 两种

	- 2、通过select函数，遍历1中的socket数组，当某个socket有响应，select就会通过其参数/返回值反馈出来。

		- 我们就做相应处理

	- 3、我们就做相应处理

		- 如果检测到的是服务器socket

			- 那就是有客户端链接
			- 调用accept

		- 如果检测到的是客户端socket

			- 那就是客户端请求通信
			- send或者recv

- 第一步

	- 定义一个装客户端socket结构

		- fd_set  clientSet;

			- fd_set网络定义好的专门给咱们用的
			- 转定义看下fd_set的声明

				- 64

					- 默认FD_SETSIZE是64, 大家可以在Winsock2.h前声明这个宏，给他更大的值，他就更大了，不要过大
					- 因为原理就是不听的遍历检测，越多肯定效率越低，延迟越大，所以大家尽量不要太大，几百个，1024就差不多了，当然大家不怕慢，可以设置更多~
					- 所以，select模型应用，就是小用户量访问量，几十几百，简单方便。

		- 四个操作fd_set的参数宏

			- 依次转定义看下逻辑
			- FD_ZERO

				- 讲集合清0
				- FD_ZERO(&clientSet);

			- FD_SET

				- 向集合中添加一个socket

					- 当数量不足64，并且不存在的时候

				- FD_SET(socket, &setRead);

			- FD_CLR

				- 结合中删除指定socket
				- FD_CLR(socket, &setRead);

					- 我们要手动释放

			- FD_ISSET

				- 判断一个socket是否在集合中

					- 不在返回0
					- 在返回非0

				- FD_ISSET(socket, &setRead)

- 第二步

	- select

		- 作用

			- 监视socket集合，如果某个socket发生事件（链接或者收发数据），通过返回值以及参数告诉我们

		- 函数原型

			- int WSAAPI select
(
  int           nfds,
  fd_set        *readfds,
  fd_set        *writefds,
  fd_set        *exceptfds,
  const timeval *timeout
);

				- 参数1

					- 忽略

						- 填0

					- 这个参数仅仅是为了兼容Berkeley sockets.

				- 参数2

					- 检查是否有可读的socket

						- 即客户端发来消息了，该socket就会被设置

					-  &setRead

						- 初始化为所有的socket，通过select投放给系统，系统将有事件发生的socket再赋值回来，调用后，这个参数就只剩下有请求的socket

				- 参数3

					- 检查是否有可写的socket

						- 就是可以给哪些客户端套接字发消息，即send
						- 只要链接成功建立起来了，那该客户端套接字就是可写的

					-  &setWrite

						- 初始化为所有的socket，通过select投放给系统，系统将可写的socket再赋值回来，调用后，这个参数就是装着可以被send数据的客户端socket
						- 一般我们就直接send了，所以这个参数逻辑上，用的不是非常多

				- 参数4

					- 检查套接字上的异常错误

						- 用法跟2 3一样
						- 将有异常错误的套接字重新装进来，，反馈给我们

					- 得到异常套接字上的具体错误码

						-  getsockopt （socket，SOL_SOCKET，  SO_ERROR, buf , buflen）

							- 错误信息通过参数4返给我们

				- 参数5

					- 最大等待时间

						- 比如当客户端没有请求时，那么select函数可以等一会儿，一段时间过后，还没有，就继续执行select下面的语句，如果有了，就立刻执行下面的语句

					-  TIMEVAL 

						- 两个成员

							- tv_sec 

								- 秒

							- tv_usec 

								- 微秒

						- 0 0

							- 非阻塞状态，立刻返回

						- 3 4

							- 那就再无客户端响应的情况下等待3秒4微秒

					- NULL

						- select完全阻塞

							- 直到客户端有反应，我才继续

				- 返回值

					- 0

						- 客户端在等待时间内没有反应
						- 处理

							- continue就行了

					- >0

						- 有客户端请求交流了

					-  SOCKET_ERROR

						- 发生了错误
						-  得到错误码WSAGetLastError()

- 第四步

	- >0处理

		- 客户端socket有事件

			- 可读的

				- 遍历响应socket集合

					- 如果是socketServer

						- accept

					- 客户端socket

						- recv

							- 0

								- 客户端正常下线

									- 要在socket集合中删除对应的客户端socket

							- >0

								- 客户端发来了消息，buf中

							- socket_error

								- 客户端有错误发生

			- 可写的

				- 调用send

					- 成功返回发送的字节数
					- 失败返回socket_error

						- WSAGetLastError的到错误码

			- 有异常的

				-  getsockopt （socket，SOL_SOCKET，  SO_ERROR, buf , buflen）

					- 作用

						- 将socket上产生的异常，通过参数4返回给咱们

					- 返回值

						- 成功返回0
						- 失败返回sock_error

							- WSAGetLastError得到错误码

### 总结

- 代码结构

	- select模型的代码，也会有细节优化的，结构稍有变化的，但是原理都一样，咱们讲的就是最笨，思路最简单的方法，让大家理解这个模型的处理本质

- 结构核心

	- 参数2

		- 处理accept与recv傻等的问题

			- 这是select结构对比基本模型的最大的作用
			- 所以大家看网上的一些代码，基本都是只有参数2，参数3，4都是NULL

	- 参数3

		- send随时都能发，并不一定由参数3决定send

- 流程总结

	- socket集合

		- select判断有没有响应的

			- 返回0：没有，继续挑
			- 返回>0：有响应的

				- 可读的

					- accept
					- recv

				- 可写的

					- send

						- 不是非得从此处调用send

				- 异常的

					- getsockopt

			- sock_error

- select是阻塞的

	- 不等待

		- 执行阻塞

	- 半等待

		- 执行阻塞+软阻塞

	- 全等待

		- 执行阻塞+硬阻塞

			- 死等

### 释放socket

- 释放所有socket
- 控制台点x退出

	- 主函数投递一个监视

		- SetConsoleCtrlHandler(HandlerRoutine, FALSE);

	- 处理函数

		- BOOL WINAPI HandlerRoutine(DWORD dwCtrlType )
{
    switch (dwCtrlType)
    {
        case CTRL_CLOSE_EVENT:
        {
            释放socket;
        }	
    }
    return TRUE;
}

```cpp
// select.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include<iostream>
#define FD_SETSIZE 1024
#include<WinSock2.h>
#include<windows.h>




#pragma comment(lib,"ws2_32.lib")

using namespace std;
SOCKET serverSocket;
fd_set allSocket;

BOOL WINAPI ExitFun(DWORD dwCtrlType) {

	switch (dwCtrlType)
	{
	case CTRL_CLOSE_EVENT:
		FD_CLR(serverSocket, &allSocket);
		for (u_int i = 0; i < allSocket.fd_count; i++) {
			closesocket(allSocket.fd_array[i]);
		}
		FD_ZERO(&allSocket);
		closesocket(serverSocket);
		WSACleanup();
		break;
	}
	return TRUE;
}

int main(int argc, char *argv[])
{
	SetConsoleCtrlHandler(ExitFun, TRUE);

	WORD wdVersion = MAKEWORD(2, 2);

	WSADATA wdSockMsg;

	int nRes = WSAStartup(wdVersion, &wdSockMsg);

	if (0 != nRes) {
		cout << "WSAStartup Error" << endl;
		return 0;
	}

	if (HIBYTE(wdSockMsg.wVersion) != 2 || LOBYTE(wdSockMsg.wVersion) != 2)
	{
		WSACleanup();
		cout << "网络库版本出错" << endl;
		return 0;
	}

	serverSocket = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);

	if (serverSocket == INVALID_SOCKET) {
		cout << "socket Error: " << WSAGetLastError() << endl;
		WSACleanup();
		return 0;
	}

	sockaddr_in si;
	si.sin_family = AF_INET;
	si.sin_port = htons(12346);
	si.sin_addr.S_un.S_addr = inet_addr("192.168.1.103");

	if (SOCKET_ERROR == bind(serverSocket, (sockaddr *)&si, sizeof(si)))
	{
		cout << "bind Error: " << WSAGetLastError() << endl;
		closesocket(serverSocket);
		WSACleanup();
		return 0;
	}

	if (SOCKET_ERROR == listen(serverSocket,SOMAXCONN))
	{
		cout << "listen Error: " << WSAGetLastError() << endl;

		closesocket(serverSocket);
		WSACleanup();
		return 0;
	}

	//初始化allSocket变量
	FD_ZERO(&allSocket);
	//将服务器套接字添加到allSocket中
	FD_SET(serverSocket,&allSocket);

	while (true) {
		//设置等待时间
		TIMEVAL waitTime;
		waitTime.tv_sec = 3;
		waitTime.tv_usec = 0;

		//使用临时的fd_set变量,防止被覆盖掉
		fd_set readAllSocket = allSocket;
		fd_set writeAllSocket = allSocket;
		fd_set exAllSocket = allSocket;
		
		//进入select,将有消息的套接字选出来
		int nStRes = select(0, &readAllSocket, &writeAllSocket, &exAllSocket, &waitTime);

			//没有消息
			if (nStRes == 0) {
				continue;
			}
			//有消息
			else if (nStRes > 0)
			{
				//处理有请求的套接字
				for (u_int i = 0; i < readAllSocket.fd_count; i++)
				{
					//如果是服务器套接字,说明有accept请求
					if (readAllSocket.fd_array[i] == serverSocket) {
						sockaddr_in clientAddr = { 0 };
						int addrLen = sizeof(clientAddr);
						SOCKET clientSocket = accept(serverSocket,(sockaddr *)&clientAddr,&addrLen);
						if (clientSocket == INVALID_SOCKET)
						{
							cout << "accept Error: " << WSAGetLastError() << endl;
							continue;
						}
						if (allSocket.fd_count == FD_SETSIZE) {
							cout << "服务器已满" << endl;
							send(clientSocket, "服务器爆满,请稍后连接...", sizeof("服务器爆满,请稍后连接...\n"), 0);
							closesocket(clientSocket);
							continue;
						}

						cout << "连接服务器成功 : " << inet_ntoa(clientAddr.sin_addr) << endl;
						int sendRes = send(clientSocket, "连接成功...", sizeof("连接成功...") + 1,0);
					
						FD_SET(clientSocket, &allSocket);

					}
					//收消息
					else {
						char recBuf[1500] = { 0 };
						int recvRes = recv(readAllSocket.fd_array[i], recBuf, sizeof(recBuf), 0);

						if (recvRes == SOCKET_ERROR)
						{
							int WSAError = WSAGetLastError();
							cout << "recv Error: " << WSAError << endl;
							if (WSAError == 10054) {
								cout << "客户端强制下线了" << endl;
								FD_CLR(readAllSocket.fd_array[i],&allSocket);
								continue;
							}
						}
						else if (recvRes == 0) {
							cout << "客户端下线了" << endl;
							FD_CLR(readAllSocket.fd_array[i], &allSocket);
							continue;
						}
						else {
							cout << recBuf << endl;
						}
					}




				}//for readAllsocket

				//处理异常错误
				for (u_int i = 0; i < exAllSocket.fd_count; i++)
				{
					char exBuf[1024] = { 0 };
					int exLen = 1024;
					getsockopt(exAllSocket.fd_array[i], SOL_SOCKET, SO_ERROR, exBuf, &exLen);
					cout <<"异常错误: " << exBuf<< endl;

				}// for exAllSocket

			}//else if 有消息

			//select出错了
			else {
				cout << "select Error: " << WSAGetLastError() << endl;

			}

	}//while(true)

	for (u_int i = 0; i < allSocket.fd_count; i++) {
		closesocket(allSocket.fd_array[i]);
	}
	closesocket(serverSocket);
	WSACleanup();
    return 0;
}


```

