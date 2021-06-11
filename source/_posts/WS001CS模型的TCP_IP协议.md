---
title: TCP/IP协议
date: 2021-05-28 09:31:12
tags:
categories: Windows网络编程
doc:
---



# TCP/IP协议

## 基于tcp/ip协议的c/s模型

### tcp/ip协议

- Transmission Control Protocol / Internet Protocol
- 重要性

  - TCP/IP是今天的互联网的基石，没有这个就上不了网了

- 概念

  - tcp/ip协议族(簇，组，体系），并不是tcp协议和ip协议的总称，tcp/ip指的是整个网络传输体系。而tcp协议和ip协议就是单单的两个协议。

- tcp/ip协议的特点

  - 面向连接的，可靠的，基于字节流的传输层协议。
  - udp/ip

    - 面向非连接的，不可靠的，基于数据报的传输层协议。

### client/server ----- 客户端/服务器模型

- 比如QQ，DNF，LOL等这些我们下载客户端的，都属于c/s模型的一个应用
- c/s模型其实是概念层面的，实现层面可以是基于任何的网络协议。
- 常见的还有b/s模型

  - 浏览器/服务器模型   基于http/https协议的

### 套接字编程与socket编程

- socket中文->套接字，也统称叫网络编程

### 演示我们要实现的效果

- 局域网
- 如果在广域网使用啊

  - 内网穿透，内网转发

    - 花生壳
    - Sunny-Ngrok
    - ...

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

- ```
  int WSAStartup(
  WORD      wVersionRequired,
  LPWSADATA lpWSAData
  );
  ```

  

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

        - ```
          struct WSAData {
          WORD           wVersion;
          WORD           wHighVersion;
          unsigned short iMaxSockets;
          unsigned short iMaxUdpDg;
          char           *lpVendorInfo;
          char           szDescription[WSADESCRIPTION_LEN + 1];
          char           szSystemStatus[WSASYS_STATUS_LEN + 1];
          } 
          ```

          

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
        -  释放

           - closesocket(socketListen);
           - WSACleanup();

### 创建客户端socket/接受连接

- SOCKET WSAAPI accept(
  SOCKET   s,
  sockaddr *addr,
  int      *addrlen
  );

  - 作用

    - accept函数允许在套接字上进行传入连接尝试。
    - listen监听客户端来的链接，accept讲客户端的信息绑定到一个socket上，也就是给客户端创建一个socket,通过返回值返回给我们客户端的socket
    - 一次只能创建一个，有几个客户端链接，就要调用几次

  - 参数1

    - 我们上面创建的自己的socket

      - socket先处于监听状态，然后来的链接都在由这个管理，我们取客户端的信息，就是通过这个我们自己的socket

  - 参数2

    - 客户端的地址端口信息结构体

      - 跟bind的第二个参数一样

        - 意义：系统帮我们监视着客户端的动态，肯定会记录客户端的信息，也就是IP地址，和端口号，并通过这个结构体记录

      - SOCKADDR_IN sockClient

        - 这个我们不用填写，系统帮我们填写，也即传址调用

      - 参数2 3也能都设置成NULL，那就是不直接得到客户端的地址，端口号咯

        - 此时可以通过函数得到客户端信息

          - getpeername(newSocket, (struct sockaddr*)&sockClient, &nLen);

        - 得到本地服务器信息

          - getsockname(sSocket, (sockaddr*)&addr, &nLen);

  - 参数3

    - 参数2的大小

      - sizeof

  - 返回值

    - 成功

      - 返回值就是给客户端包好的socket

        - 与客户端通信就靠这个

    - 失败

      - 返回INVALID_SOCKET 

        - WSAGetLastError()得到错误码
        - 释放空间

          - closesocket(socketListen);
          - WSACleanup();

  - accept调试

    - 1、阻塞，同步

      - 这个函数是阻塞的，没有客户端链接，那就一直卡在这儿，等着。

    - 2、多个链接

      - 一次只能一个，5个就要5次循环

    - 理解缺点

### 与客户端收发消息

- 收

  - int recv(
    SOCKET s,
    char   *buf,
    int    len,
    int    flags
    );

    - 作用

      - 得到指定客户端（参数1）发来的消息

    - 原理

      - 本质：复制

        - 数据的接收都是由协议本身做的，也就是socket的底层做的，系统会有一段缓冲区，存储着接收到的数据。
        - 咱们外边调用recv的作用，就是通过socket找到这个缓冲区，并把数据复制进咱们的参数2，复制参数3个

    - 参数1

      - 客户端的socket，每个客户端对应唯一的socket

    - 参数2

      - 客户端消息的存储空间，也就是个字符数组

        - 这个一般1500字节

          - 网络传输得最大单元，1500字节，也就是客户端发过来得数据，一次最大就是1500字节，这是协议规定，这个数值也是根据很多情况，总结出来得最优值
          - 所以客户端最多一组来1500字节，咱们这头1500读一次，够够的了。

    - 参数3

      - 想要读取得字节个数

        - 一般是参数2得字节数-1，把\0字符串结尾留出来

    - 参数4

      - 数据的读取方式

        - 0
        - 正常逻辑来说

          - 我们从系统缓冲区把数据读到我们的buf,读到我们buf中后，系统缓冲区的被读的就应该被删除掉了，不然也是浪费空间，毕竟，通信时间长的话，那就爆炸了
          - 我们将缓冲区的数据读到我们自己的buf，根据需要处理相应的数据，这是我们可控的，完全玩儿弄于咱么你自己的鼓掌，系统缓冲区的数据，咱们无可奈何，操作不了
          - 读出来的就删除的话，有很多的好处

            - 1、系统缓冲区读到的数据，比我们的buf多，那么我们读出来的，系统删掉，从而我们就可以依次的把所有数据读完了

              - 比如

                - 系统缓冲区收到abcdefghijk，咱们的recvbuf一次读4个字节，那么我们放循环里，就会依次读出abcd,efgh,ijk

              - 如果不删，那每次都是从头读

                - 在循环里就是每次都是abcd......只读到这四个

            - 2、可以计数收到了多少字节

              - 返回值就是本次读出来的数据

          - 正常这种逻辑

            - 填0

              - 哈哈
              - 读出来的就删除

        - MSG_PEEK

          - 窥视传入的数据。 数据将复制到缓冲区中，但不会从输入队列中删除。

            - 读出来的不删除

          - 这个东西是不建议被使用的

            - 第一、读数据不行
            - 第二、那就无法计数了

        - MSG_OOB

          - 带外数据

            - 意义

              - 就是传输一段数据，在外带一个额外的特殊数据

                - 相当于小声BB

            - 实际

              - 就不建议被使用了

                - 1、TCP协议规范（RFC 793）中OOB的原始描述被“主机要求”规范取代（ RFC 1122），但仍有许多机器具有RFC 793 OOB实现。
                - 2、既然两种数据，那咱们就send两次，另一方recv两次就行了，何必搞得那么神神秘秘，浪费计算机精力

        - MSG_WAITALL

          - 直到系统缓冲区字节数满足参数3所请求得字节数，才开始读取

    - 返回值

      - 读出来字节数大小

        - 读没了咋办？

          - 在recv函数卡着，等着客户端发来数据

            - 即阻塞，同步

          - 阻塞的

      - 客户端下线，这端返回0

        - 释放客户端socket

      - 执行失败，返回SOCKET_ERROR

        - WSAGetLastError()得到错误码
        - 根据错误码信息做相应处理

          - 重启
          - 等待
          - 不用理会

- 发

  - int WSAAPI send(
    SOCKET     s,
    const char *buf,
    int        len,
    int        flags
    );

    - 作用

      - 向目标发送数据
      - 本质

        - send函数将我们的数据复制黏贴进系统的协议发送缓冲区，计算机伺机发出去
        - 最大传输单元是1500字节

    - 参数1

      - 目标的socket，每个客户端对应唯一的socket

    - 参数2

      - 给对方发送的字节串

        - 这个不要超过1500字节

          - 发送时候，协议要进行包装，加上协议信息，也叫协议头，或者叫包头，咱们在理论部分，会非常详细的介绍协议头，以及功能
          - 这个大小不同的协议不一样，链路层14字节，ip头20字节，tcp头20字节，数据结尾还要有状态确认，加起来也几十个字节，所以实际咱们的数据位，不能写1500个，要留出来，那就1024吧，或者最多1400，就差不多了

            - 懂这个大体原理就好
            - 详细的包的封装原理，我们在后面纯理论部分介绍

          - 当然大家这个不一定每次都是正好那么多字节，比如聊天，一句话就十个八个的汉字，别多于1400是最好的

        - 超过1500系统咋办？

          - 系统会分片处理

            - 比如2000个字节
            - 系统分成两个包

              - 1400+包头==1500

                - 假设包头100字节

              - 600+包头==700
              - 分两次发送出去

            - 结果

              - 1、系统要给咱们分包再打包，再发送， 客户端接收到了还得拆包，组合数据。从而增加了系统的工作，降低效率
              - 2、有的协议，就把分片后的二包直接丢了

    - 参数3

      - 字节个数

        - 1400

    - 参数4

      - 写0就行了
      - 其他

        - MSG_OOB

          - 意义同recv

            - 就不用使用了

          - 带外数据

            - 意义

              - 就是传输一段数据，在外带一个额外的特殊数据

                - 相当于小声BB

            - 实际

              - 就不建议被使用了

                - 1、TCP协议规范（RFC 793）中OOB的原始描述被“主机要求”规范取代（ RFC 1122），但仍有许多机器具有RFC 793 OOB实现。
                - 2、既然两种数据，那咱们就send两次，另一方recv两次就行了，何必搞得那么神神秘秘，浪费计算机精力

        - MSG_DONTROUTE

          - 指定数据不应受路由限制。 Windows套接字服务提供程序可以选择忽略此标志。

    - 返回值

      - 成功返回写入的字节数
      - 执行失败，返回SOCKET_ERROR

        - WSAGetLastError()得到错误码
        - 根据错误码信息做相应处理

          - 重启
          - 等待
          - 不用理会

## 客户端

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

### 链接到服务器

- int WSAAPI connect
  (
  SOCKET         s,
  const sockaddr *name,
  int            namelen
  );

  - 作用

    - 链接服务器并把服务器信息与服务器socket绑定到一起

  - 参数1

    - 服务器socket

  - 参数2

    - 服务器Ip地址端口号结构体

  - 参数3

    - 参数2结构体大小

  - 返回值

    - 成功

      - 返回0

    - 失败

      - 返回 SOCKET_ERROR

        - WSAGetLastError()得到错误码
        - 释放空间

          - closesocket(socketListen);
          - WSACleanup();

### 与服务器收发消息

- 收

  - recv(newSocket, szRecvBuffer, sizeof(szRecvBuffer), 0);

    - 参数1
    - 参数2
    - 参数3
    - 参数4
    - 返回值

- 发

  - send(newSocket, szSendBuffer, strlen(szSendBuffer)+1, 0);

    - 参数1
    - 参数2
    - 参数3
    - 参数4
    - 返回值

## 思考

### 加个循环，稍微完善下逻辑

### 问题

- 由于accept  recv是阻塞的，做其中一件事，另外一件事就做不了，所以假设有多个客户端的情况下，
- 我们当前的模型，我先等，我不管及将来的是什么请求，我先等

  - 结果：咱们在这等收消息recv，结果来了个链接请求，那就无法处理，链接只能accept
  - 另外，我们等的socket可能没有发请求，那我们完了，等到睡着。
  - 从而，我这个服务器，就是废了

### 分析

- 我们直接主动跟系统要，要什么？要有请求的socket，哪个有请求了，就给我哪个

  - 结果：
    得到链接请求，我们就直接accept
    得到是发来了消息，我们就recv
  - 从而就不会发生无谓的傻等情况

### 得到

- select模型

  - select就是挑选的意思，它把请求的套接字给我们选出来，我们直接就去处理这些套接字
  - 即：select就是处理accept与recv阻塞问题的

    - send本身也是阻塞的，不是长阻塞，是短阻塞

