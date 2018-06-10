### 前言

实习的时候发现自己网络知识非常匮乏，所以趁着回学校有时间重新看<<计算机网络>> 和 <<tcp/ip>>第一章。

### 因特网的工作方式

 1. 客户-服务方式： 因特网最常用的方式。客户-服务指的是进程之间服务与被服务之间的关系。对于客户端而言：用户后台运行，在通信时主动向远方的服务器发起通信，因此客户端必须知道服务器程序地址。对于服务而言：系统启动之后自动调用并不断的运行，被动地等待接收客户的通信请求，可同时处理多个客户的请求。

 2. 对等连接方式（P2P) :两个主机通信时并不区分哪个是服务请求方还是服务提供方，双方进行对等连接通信。用户使用P2P软件时，下载不是从一个地方下载所有的文件。当用户在下载时，又把自己的文件上传给其他用户，所以使用的人数越多，下载速度越快。
 
### TCP/IP协议分层
TPC/IP通常被认定成一个四层协议系统，如图

![layer]({{ site.baseurl }}/images/layer.png)

1. 链路层： 有时称为数据链路层，通常包括操作系统中的设备驱动和计算机对应网络接口卡。他们一起处理与传输媒介的物理接口细节。

    * 适配器：用来进行数据串行传输和并行转换。适配器在接收数据时并不适用计算机CPU,当适配器接收到正确帧时，产生中断通知交付给网络层。
    * 以太网适配器还可以设置成**混杂模式**，工作在混杂模式下的适配器会接收到所有"听到"的帧。
    * MAC地址：又称为硬件地址或者物理地址。MAC是固化在适配器中的6个字节。
    * 单播帧：收到的帧的MAC地址与本站的硬件地址相同
	* 广播帧：发送给本局域网所有站点的帧（MAC地址全为1）
	* 多播帧：发送给本局域网上一部分站点的帧
    * ARP协议： 在局域网内发送广播报文，请求其他站点的IP和对应的MAC地址。
    
2. 网络层： 有时也称为互联网层，处理分组在网络中的活动，例如分组的选择。在TCP/IP协议族中，网络层协议包括IP协议（网际协议），ICMP协议（Internet互联网控制报文协议),以及IGMP协议（Interner组管协议）。

 
    * ICMP协议： 允许ICMP主机报告差错情况和提供有关异常情况的报告。其中差错报文有5种：终点不可达、源点抑制、参数问题、改变路由。
    
3. 运输层主要为两台主机之间的应用程序提供端到端的通信。在TCP/IP协议中，有两个不用的传输协议： TCP(传输控制协议）和 UDP（用户数据包协议）。TCP为两台主机提供高可靠的数据通信。它所做的工作把应用程序交给它的数据分成合适的小块交给下面的网络层，确认接收到的分组，设置发送最后确认的分组的超时时间等。由于运输层提供高可靠性的端到端的通信。它只是把称作数据报的分组从一台主机发送到另一台主机上，并不保证该数据能到到另一端。任何必须的可靠性必须由应用层来完成。

4. 应用层负责处理特定的应用程序细节。几乎各种不同的TCP/IP实现都提供以下通用的应用程序：
	* Telent 远程登录
	* FTP 文件传输协议
	* SMTP 简单邮件传送协议
	* POP3 邮件读取协议
	* SNMP 简单网络管理协议

### ip数据包格式
	![ip]({{ site.baseurl }}/images/ip.png)
### TCP数据包格式
	![TCP]({{ site.baseurl }}/images/TCP.png)
### UDP数据包格式
	![UDP]({{ site.baseurl }}/images/udp.png)
### 三次握手
	![connect]({{ "/css/pics/connect.png"}}) 
   * 同步SYN ：在连接建立时用来同步序号。当SYN = 1 而 ACK = 0 时表明这是一个连接请求。对方如果同意建立连接，在响应报文中 ACK = 1 SYN = 1。

   * ack 确认号：是期望收到对方下一个报文段中的第一个数据字节序的序号。
确认ACK： 仅当 ACK = 1 时，确认号ack才有用。连接建立后所有传送报文都必须把ACK置1.
	
### 为什么clinet还要发送一次确认？
主要是防止失效的连接请求报文段突然有传送到server,因此产生错误。
假设：Client发出一个连接请求，但是该连接阻塞在网络中，没有收到回应，所以Client重新发送连接请求，并收到回应，此时连接建立，并传输数据，释放连接。Client发送了两次连接请求报文，若此时第一次发送连接请求延时后重新到达Server,此时Server回应Client，同意建立连接，假如不采用三次握手的话，此时又重新建立了错误的连接。 而采用三次握手，Client收到Server的响应后，察觉报文已失效，不发送确认。
### 四次挥手的必要性
连接是双向的，四次握手让双方都正常进入close状态。
	
### 系统调用和应用编程接口
当某个应用进程启动系统调用时，控制权就从应用进程传递给了系统调用接口。此接口再把控制权传递给计算机操作系统。操作系统再把这个调用转给某个内部过程，并执行所请求的操作。内部操作一旦执行完成之后，控制权就又通过系统调用返回给应用程序。总之，应用进程需要从操作系统中获得服务，就要把控制权传递给操作系统，操作系统在执行必要的操作后把控制权返回给应用进程。