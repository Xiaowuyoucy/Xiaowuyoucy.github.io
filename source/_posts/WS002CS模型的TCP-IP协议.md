---
title: CS模型的TCP/IP协议
date: 2021-06-08 22:48:18
tags:
categories: Windows网络编程
doc:
---

# CS模型的TCP/IP协议

### 创建服务端步骤

1. 打开网络头文件
2. 启动网络库
3. 校验版本
4. 创建SOCKET
5. 绑定地址与端口
6. 开始监听
7. 接受连接
8. 与客户端收发消息

### 网络头文件 网络库

```
#include <WinSock2.h> 
#pragma comment(lib,“ws2_32.lib”)  
```

目前网络库有哪些版本:

1.0
 1.1
 2.0
 2.1
 2.2

### WSADATA结构

```
typedef struct WSAData {
        WORD                    wVersion;
        WORD                    wHighVersion;
#ifdef _WIN64
        unsigned short          iMaxSockets;
        unsigned short          iMaxUdpDg;
        char FAR *              lpVendorInfo;
        char                    szDescription[WSADESCRIPTION_LEN+1];
        char                    szSystemStatus[WSASYS_STATUS_LEN+1];
#else
        char                    szDescription[WSADESCRIPTION_LEN+1];
        char                    szSystemStatus[WSASYS_STATUS_LEN+1];
        unsigned short          iMaxSockets;
        unsigned short          iMaxUdpDg;
        char FAR *              lpVendorInfo;
#endif
} WSADATA;
```

**wVersion**

Windows Sockets DLL期望调用者使用的Windows Sockets规范的版本。 高位[字节](https://baike.baidu.com/item/字节)存储副版本号, 低位字节存储主版本号，可以用WORD MAKEWORD(BYTE,BYTE ) 返回这个值,例如:MAKEWORD(1,1)

**wHighVersion** 

这个DLL能够支持的Windows Sockets规范的最高版本。通常它与wVersion相同。

**szDescription** 

以null结尾的ASCII字符串，Windows Sockets DLL将对Windows Sockets实现的描述拷贝到这个字符串中，包括制造商标识。文本（最多可以有256个字符）可以包含任何字符，但是要注意不能包含[控制字符](https://baike.baidu.com/item/控制字符)和[格式字符](https://baike.baidu.com/item/格式字符)，应用程序对其最可能的使用方式是把它（可能被截断）显示在在状态信息中。

**szSystemStatus** 

以null结尾的ASCII字符串，Windows Sockets DLL把有关的状态或配置信息拷贝到该字符串中。Windows Sockets DLL应当仅在这些信息对用户或支持人员有用时才使用它们，它不应被作为szDescription域的扩展。

**iMaxSockets**

单个进程能够打开的socket的最大数目。Windows  Sockets的实现能提供一个全局的socket池，可以为任何进程分配；或者它也可以为socket分配属于进程的资源。这个数字能够很好地反映Windows Sockets DLL或网络软件的配置方式。应用程序的编写者可以通过这个数字来粗略地指明Windows  Sockets的实现方式对应用程序是否有用。例如，X  Windows服务器在第一次启动的时候可能会检查iMaxSockets的值：如果这个值小于8，应用程序将显示一条错误信息，指示用户重新配置网络软件（这是一种可能要使用szSystemStatus文本的场合）。显然无法保证某个应用程序能够真正分配iMaxSockets个socket，因为可能有其它WindowsSockets应用程序正在使用。

**iMaxUdpDg**

Windows Sockets应用程序能够发送或接收的最大的用户数据包协议（UDP）的数据包大小，以[字节](https://baike.baidu.com/item/字节)为单位。如果实现方式没有限制，那么iMaxUdpDg为零。在Berkeley sockets的许多实现中，对于UDP数据包有个固有的限制（在必要时被分解），大小为8192字节。Windows  Sockets的实现可以对碎片重组缓冲区的分配作出限制。对于适合的WindowsSockets  实现，iMaxUdpDg的最小值为512。注意不管iMaxUdpDg的值是什么，都不推荐你发回一个比网络的最大传送单元（MTU）还大的[广播数据包](https://baike.baidu.com/item/广播数据包)。（Windows Sockets API 没有提供发现MTU的机制，但是它不会小于512个字节)。WinSock2.0版中已被废弃。

**lpVendorInfo**

指向销售商的数据结构的[指针](https://baike.baidu.com/item/指针)。这个结构的定义（如果有）超出了[WindowsSockets规范](https://baike.baidu.com/item/WindowsSockets规范)的范围。WinSock2.0版中已被废弃。

###  WSAStartup打开网络库

```
int WSAStartup(
WORD      wVersionRequired,
LPWSADATA lpWSAData
);
```

⑴ wVersionRequested：一个WORD（双字节）型数值，在最高版本的Windows Sockets支持调用者使用，高阶字节指定小版本(修订本)号,低位字节指定主[版本](https://baike.baidu.com/item/版本/505574)号。

⑵lpWSAData 指向WSADATA数据结构的[指针](https://baike.baidu.com/item/指针)，用来接收Windows Sockets [1] 实现的[细节](https://baike.baidu.com/item/细节/82732)。

WindowsSockets API提供的调用方可使用的最高版本号。高位[字节](https://baike.baidu.com/item/字节)指出副版本(修正)号，低位字节指明主版本号。

返回值:

0 成功。

否则返回下列的错误代码之一。注意通常依靠应用程序调用WSAGetLastError()机制获得的[错误代码](https://baike.baidu.com/item/错误代码)是不能使用的，因为Windows Sockets DLL可能没有建立“上一错误”信息储存的客户数据区域。



错误代码:

WSASYSNOTREADY 代码数值为10091，指出网络通信依赖的网络子系统还没有准备好。

WSAVERNOTSUPPORTED 代码数值为10092，表示所需的Windows Sockets API的版本未由特定的Windows Sockets实现提供。

WSAEINVAL 代码数值为10022，说明应用程序指出的Windows Sockets版本不被该DLL支持。

WSAEINPROGRESS 代码数值为10036，说明一个阻塞的Winsock调用正在进行中。

WSAEPROCLIM 代码数值为10067，说明已经达到了Windows Sockets实现所支持的任务数量的极限。

WSAEFAULT 代码数值为10014，说明lpWSADATA参数是一个无效的指针。

```
//MAKEWORD(主版本,副版本)   负责将2,2分配到wdVerSion低8位和高8位
//wdVerSion低8位存储主版本,高8位存储副版本
WORD wdVerSion = MAKEWORD(2, 2);
WSADATA wdSockMsg;

int nRes = WSAStartup(wdVerSion, &wdSockMsg);

if (0 != nRes)
{
	printf("WSAStartup Error %d\n",nRes);
}
```



### 校验版本

```
//校验网络库版本
//HIBYTE取高8位,LOBYTE取高8位
if (HIBYTE(wdSockMsg.wVersion) != 2 || LOBYTE(wdSockMsg.wVersion) != 2)
{
	//关闭网络库
	WSACleanup();
	return 0;
}
```



### 创建SOCKET套接字



```
SOCKET PASCAL FAR socket( int af, int type, int protocol);
```

参数1

地址的类型

- 比如大家联系我

  - 手机 15512345678
  - 固定电话 7881234
  - ＱＱ 40916626xx
  - 微信 c3_xin666
  - 找上门  内蒙古 xxxxxxx
  - …

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

  <br />

参数2

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

    <br />

参数3

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

返回值

- 成功返回可用的socket

  - 不用了就一定要销毁套接字

    - closesocket(socketListen);

      

- 失败返回INVALID_SOCKET

  - 关闭网络库

    - WSACleanup();

      

  - 可用WSAGetLasterror()返回错误码

```c
	SOCKET sock = socket(AF_INET,SOCK_STREAM,IPPROTO_TCP);
	//如果创建套接字失败
	if (INVALID_SOCKET == sock)
	{
		printf("socket Error: %d\n",WSAGetLastError());
		//关闭网络库
		WSACleanup();
	}
```

<br>

### 绑定地址与端口

给我们的socket绑定端口号与具体地址

```
int PASCAL FAR bind( SOCKET sockaddr, const struct sockaddr FAR* my_addr,int addrlen);
```

参数1
	上一个函数创建了socket，绑定了协议信息（地址类型，套接字类型，协议类型），咱们bind函数就是绑定实质的地址，端口号

参数2

```
sockaddr_in sockAddress;
sockAddress.sin_family = AF_INET; //地址类型
sockAddress.sin_addr.s_addr = inet_addr("127.0.0.1"); //服务器ip地址
sockAddress.sin_port = 12345;	//服务器端口号
(sockaddr*)&sockAddress强转添加到参数2上
```



参数3

参数2的类型大小

sizeof(sockAddress)

返回值
	成功返回0
	失败返回SOCKET_ERROR
		具体错误码通过int WSAGetLastError(void);获得 
		closesocket(socketListen);
WSACleanup();



##### htons

 htons的功能：将一个无符号短整型的主机数值转换为网络[字节顺序](https://baike.baidu.com/item/字节顺序)，即大尾顺序(big-endian) 

```
u_short htons(u_short hostshort);
```

而我们常用的 x86 CPU (intel, AMD) 电脑是 little-endian,也就是整数的低位[字节](https://baike.baidu.com/item/字节)放在内存的低字节处。举个例子吧。假定你的数据是0x1234,

在网络字节顺序里 这个数据放到内存中就应该显示成

addr addr+1

0x12 0x34

而在x86电脑上，数据0x1234放到内存中实际是：

addr addr+1

0x34 0x12

htons 的用处就是把实际主机内存中的整数存放方式调整成网络字节顺序。

##### inet_addr

 inet_addr是一个[计算机函数](https://baike.baidu.com/item/计算机函数/3504881)，功能是将一个点分[十进制](https://baike.baidu.com/item/十进制/6521392)的IP转换成一个长整数型数（u_long类型）等同于inet_addr()。 

```
in_addr_t inet_addr(const char* strptr);
```

```
daddr.s_addr=inet_addr("192.168.1.60");
```

##### inet_ntoa

功能是将网络地址转换成“.”点隔的字符串格式。 

 网络字节序IP转化点分十进制IP 

```
char *inet_ntoa (struct in_addr); 
```



```
struct sockaddr_in si;
	//地址类型
	si.sin_family = AF_INET;
	//端口
	si.sin_port = htons(12345);
	//地址
	si.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");

	if (SOCKET_ERROR == bind(sock, (const struct sockaddr *)&si, sizeof(si)))
	{
		printf("bind Error: %d\n", WSAGetLastError());
		//关闭套接字
		closesocket(sock);
		//关闭网络库
		WSACleanup();
		return 0;
	}
```



### 开始监听

将套接字置于正在侦听传入连接的状态。

```
int WSAAPI listen(
  SOCKET s,
  int    backlog
);
```



##### 参数1

​	服务器端的socket，也就是socket函数创建的

##### 参数2

​	挂起连接队列的最大长度。
​		我们一般填写这个参数
​			SOMAXCONN
​				作用是让系统自动选择最合适的个数
​				不同的系统环境不一样，所以这个合适的数也都不一样

##### 返回值

###### 	成功

​		返回0

###### 	失败

​		SOCKET_ERROR
​		具体错误码
​			 WSAGetLastError()
​			释放
​				closesocket(socketListen);
​				WSACleanup();





```
//开始监听
	if (SOCKET_ERROR == listen(sock, SOMAXCONN)) {
		printf("listen Error: %d\n", WSAGetLastError());
		//关闭套接字
		closesocket(sock);
		//关闭网络库
		WSACleanup();
		return 0;
	}
```



### 创建客户端socket/接受连接

作用
	accept函数允许在套接字上进行传入连接尝试。
	listen监听客户端来的链接，accept讲客户端的信息绑定到一个socket上，也就是给客户端创建一个socket,通过返回值返回给我们客户端的socket
	一次只能创建一个，有几个客户端链接，就要调用几次

```
SOCKET WSAAPI accept(
  SOCKET   s,	//服务端socket
  sockaddr *addr,	//用来接收客户端地址和端口
  int      *addrlen //参数2的长度
);
```

这个函数是阻塞的

参数2,参数3可以设置为NULL,不得到客户端信息,可以调用getpeername函数得到客户端信息

##### 返回值

###### 	成功

​		返回值就是给客户端包好的socket,与客户端通信就靠这个

###### 	失败

​		返回INVALID_SOCKET ,用WSAGetLastError()得到错误码
​			释放空间
​				closesocket(socketListen);
​				WSACleanup();



```
struct sockaddr_in sockClientAddr;
	int len = sizeof(sockClientAddr);
	printf("等待连接...\n");
	SOCKET sockClient = accept(sock, (struct sockaddr *)&sockClientAddr,&len);

	if (sockClient == INVALID_SOCKET) {
		printf("accept Error: %d\n", WSAGetLastError());
		//关闭套接字
		closesocket(sock);
		//关闭网络库
		WSACleanup();
	}
```



#### 通过getpeername函数得到客户端信息

```
getpeername(newSocket, (struct sockaddr*)&sockClient, &nLen);
```

#### getsockname函数得到本地服务器信息

```
getsockname(sSocket, (sockaddr*)&addr, &nLen);
```

### 与客户端收发消息

#### recv收消息

数据的接收都是由协议本身做的，也就是socket的底层做的，系统会有一段缓冲区，存储着接收到的数据。

咱们外边调用recv的作用，就是通过socket找到这个缓冲区，并把数据复制进咱们的参数2，复制参数3个

```
int recv(
  SOCKET s, //客户端socket
  char   *buf, //缓冲区
  int    len, //缓冲区大小
  int    flags  //正常写0,表示读完就删除
);
```

##### 参数1

​	客户端的socket，每个客户端对应唯一的socket

##### 参数2

​	客户端消息的存储空间，也就是个字符数组
​		这个一般1500字节
​			网络传输得最大单元，1500字节，也就是客户端发过来得数据，一次最大就是1500字节，这是协议规定，这个数值也是根据很多情况，总结出来得最优值
​			所以客户端最多一组来1500字节，咱们这头1500读一次，够够的了。

##### 参数3

​	想要读取得字节个数
​		一般是参数2得字节数-1，把\0字符串结尾留出来

##### 参数4

一般填0,表示读完就删

###### MSG_PEEK

​	窥视传入的数据。 数据将复制到缓冲区中，但不会从输入队列中删除。
​		读出来的不删除
​	这个东西是不建议被使用的
​		第一、读数据不行
​		第二、那就无法计数了

###### MSG_OOB

​	带外数据
​		意义
​			就是传输一段数据，在外带一个额外的特殊数据
​				相当于小声BB
​		实际
​			就不建议被使用了
​				1、TCP协议规范（RFC 793）中OOB的原始描述被“主机要求”规范取代（ RFC 1122），但仍有许多机器具有RFC 793 OOB实现。
​				2、既然两种数据，那咱们就send两次，另一方recv两次就行了，何必搞得那么神神秘秘，浪费计算机精力

###### MSG_WAITALL

​	直到系统缓冲区字节数满足参数3所请求得字节数，才开始读取

##### 返回值

##### 	成功

​		返回0

##### 	失败

​		SOCKET_ERROR
​		具体错误码
​			 WSAGetLastError()
​			释放
​				closesocket(socketListen);
​				WSACleanup();



### send发送消息

向目标发送数据

本质
	send函数将我们的数据复制黏贴进系统的协议发送缓冲区，计算机伺机发出去
	最大传输单元是1500字节

```
int WSAAPI send(
  SOCKET     s,			//客户端socket
  const char *buf,		//发送缓冲区
  int        len,		//发送长度
  int        flags		//一般写0
);
```

##### 参数1

​	目标的socket，每个客户端对应唯一的socket

##### 参数2

​	给对方发送的字节串
​		这个不要超过1500字节
​			发送时候，协议要进行包装，加上协议信息，也叫协议头，或者叫包头，咱们在理论部分，会非常详细的介绍协议头，以及功能
​			这个大小不同的协议不一样，链路层14字节，ip头20字节，tcp头20字节，数据结尾还要有状态确认，加起来也几十个字节，所以实际咱们的数据位，不能写1500个，要留出来，那就1024吧，或者最多1400，就差不多了
​				懂这个大体原理就好
​	
​			当然大家这个不一定每次都是正好那么多字节，比如聊天，一句话就十个八个的汉字，别多于1400是最好的
​		超过1500系统咋办？
​			系统会分片处理
​				比如2000个字节
​				系统分成两个包
​				1400 + 包头 == 1500
假设包头100字节
600 + 包头 == 700

​					分两次发送出去
​				结果
​					1、系统要给咱们分包再打包，再发送， 客户端接收到了还得拆包，组合数据。从而增加了系统的工作，降低效率
​					2、有的协议，就把分片后的二包直接丢了

##### 参数3

​	字节个数
​		1400

##### 参数4

​	写0就行了
​	其他
​		MSG_OOB
​			意义同recv
​				就不用使用了
​			带外数据
​				意义
​					就是传输一段数据，在外带一个额外的特殊数据
​						相当于小声BB
​				实际
​					就不建议被使用了
​						1、TCP协议规范（RFC 793）中OOB的原始描述被“主机要求”规范取代（ RFC 1122），但仍有许多机器具有RFC 793 OOB实现。
​						2、既然两种数据，那咱们就send两次，另一方recv两次就行了，何必搞得那么神神秘秘，浪费计算机精力
​		MSG_DONTROUTE
​			指定数据不应受路由限制。 Windows套接字服务提供程序可以选择忽略此标志。

##### 返回值

###### 	成功

返回写入的字节数

###### 	执行失败

返回SOCKET_ERROR
		WSAGetLastError()得到错误码
		根据错误码信息做相应处理
			重启
			等待
			不用理会



```c
#include <stdio.h>
#include <stdlib.h>
#include <WinSock2.h>

#pragma comment(lib,"Ws2_32.lib")

//标记
BOOL g_nFlag = TRUE;

int main(void)
{
	//打开网络库
	WORD wdVersion = MAKEWORD(1,1);
	WSADATA wsaData;
	

	//int a = WSAStartup(wdVersion, &wsaData);

	//printf("外%d\n", WSAStartup(wdVersion, &wsaData));
 	if (0 != WSAStartup(wdVersion,&wsaData)) // API 
 	{
 		printf ("WSAStartup fail!");
 		return -1;
 	}

	//校验版本
	if (1 != HIBYTE(wsaData.wVersion) || 1 != LOBYTE(wsaData.wVersion))
	{
		printf("Version fail!" );
		//关闭库
		WSACleanup();
		return -1;
	}

	//创建一个SOCKET 监听
	SOCKET socketListen = socket(AF_INET,SOCK_STREAM,0);
	if (INVALID_SOCKET == socketListen)
	{
		printf("socket fail!");
		//关闭库
		WSACleanup();
		return -1;
	}

	//绑定地址
	SOCKADDR_IN sockAddress;
	sockAddress.sin_family = AF_INET;
	sockAddress.sin_addr.s_addr = inet_addr("127.0.0.1");
	sockAddress.sin_port = htons(12345);

	if (SOCKET_ERROR == bind(socketListen,(struct sockaddr*)&sockAddress,sizeof(sockAddress)))
	{
		printf("bind fail!");
		//int nError = ::WSAGetLastError();
		//关闭库
		closesocket(socketListen);
		WSACleanup();
		return -1;
	}

	//开始监听
	if (SOCKET_ERROR == listen(socketListen,2))
	{
		printf("listen fail!");
		//关闭库
		closesocket(socketListen);
		WSACleanup();
		return -1;
	}

	//接受链接
	SOCKADDR_IN sockClient;
	int nLen = sizeof(sockClient);

	SOCKET newSocket;
	newSocket = accept(socketListen, NULL, NULL);
	//getsockname(socketListen, (struct sockaddr*)&sockClient, &nLen);
	if (INVALID_SOCKET == newSocket)
	{
		printf("listen fail!" );
		//关闭库
		closesocket(socketListen);
		WSACleanup();
		return -1;
	}


	while(g_nFlag)
	{
		//判断客户端连接的集合中是否有需要接收的数据
		char szRecvBuffer[5] = {0};
		char szSendBuffer[1024];
		//遍历setClient集合，如果发现setClient中的某个
		int nReturnValue = recv(newSocket, szRecvBuffer, sizeof(szRecvBuffer)-1, 0);
		
		int nRes = WSAGetLastError();
		if (0 == nReturnValue)
		{
			//客户端正常关闭   服务端释放Socket
			continue ;
		}
		else if (SOCKET_ERROR == nReturnValue)
		{
			//网络中断  
			printf("客户端中断连接");
			continue;
		}
		else
		{
			//接收到客户端消息 
			printf("Client Data : %s \n",szRecvBuffer);
			//给客户回信
			//scanf_s ("%s", szSendBuffer, 1024);
			//getchar();
			
			//send(newSocket, "repeat over", strlen(szSendBuffer)+1, 0);					
		}
	}


	//关闭socket
	closesocket(socketListen);
	closesocket(newSocket);
	//关闭网络库
	WSACleanup();

	//system("pause");
	return 0;
}
```



### 创建客户端步骤

1. 打开网络头文件
2. 启动网络库
3. 校验版本
4. 创建SOCKET
5. 链接到服务器
6. 与服务端收发消息

### 链接到服务器

链接服务器并把服务器信息与服务器socket绑定到一起

```
int WSAAPI connect
(
  SOCKET         s,  //服务器socket
  const sockaddr *name, //服务器Ip地址端口号结构体
  int            namelen //参数2结构体大小
);
```

##### 返回值

###### 	成功

​		返回0

###### 	失败

​		返回 SOCKET_ERROR
​			WSAGetLastError()得到错误码
​			释放空间
​				closesocket(socketListen);
​				WSACleanup();

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <Winsock2.h>
#pragma comment(lib, "Ws2_32.lib")

int main(void)
{
	WORD wdVersion = MAKEWORD(2, 2); 
	WSADATA wdScokMsg;
	int nRes = WSAStartup(wdVersion, &wdScokMsg);

	if (0 != nRes)
	{
		switch (nRes)
		{
		case WSASYSNOTREADY:
			printf("重启下电脑试试，或者检查网络库");
			break;
		case WSAVERNOTSUPPORTED:
			printf("请更新网络库");
			break;
		case WSAEINPROGRESS:
			printf("请重新启动");
			break;
		case WSAEPROCLIM:
			printf("请尝试关掉不必要的软件，以为当前网络运行提供充足资源");
			break;
		}

		return 0;
	}

	//校验版本
	if (2 != HIBYTE(wdScokMsg.wVersion) || 2 != LOBYTE(wdScokMsg.wVersion))
	{
		//说明版本不对
		//清理网络库
		WSACleanup();
		return 0;
	}

	//服务器socket
	SOCKET socketServer = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
	if (INVALID_SOCKET == socketServer)
	{
		int a = WSAGetLastError();
		//清理网络库
		WSACleanup();
		return 0;
	}

	//链接服务器
	struct sockaddr_in serverMsg;
	serverMsg.sin_family = AF_INET;
	serverMsg.sin_port = htons(12345);
	serverMsg.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");

	if (SOCKET_ERROR == connect(socketServer, (struct sockaddr*)&serverMsg, sizeof(serverMsg)))
	{
		int a = WSAGetLastError();
		closesocket(socketServer);
		//清理网络库
		WSACleanup();
		return 0;
	}

	while (1)
	{
		char buf[1500] = { 0 };
		//int res = recv(socketServer, buf, 50, 0);
		//if (0 == res)
		//{
		//	printf("链接中断、客户端下线\n");
		//}
		//else if (SOCKET_ERROR == res)
		//{
		//	//出错了
		//	int a = WSAGetLastError();
		//	//根据实际情况处理
		//}
		//else
		//{
		//	printf("%d   %s\n", res, buf);
		//}

		scanf("%s", buf);
		if ('0' == buf[0])
		{
			break;
		}
		if (SOCKET_ERROR == send(socketServer, buf, strlen(buf), 0))
		{
			//出错了
			int a = WSAGetLastError();
			//根据实际情况处理
			printf("%d\n", a);
		}
	}

	//清理网络库
	closesocket(socketServer);
	WSACleanup();

	system("pause");
	return 0;
}
```

