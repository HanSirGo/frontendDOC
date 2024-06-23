# 【网络协议】精讲OSI七层模型

![1718452072984](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452072984.png)



**1. OSI七层模型概述**

​     网络通信是现代信息社会的基石，而OSI（Open Systems Interconnection）模型是理解和设计网络系统的基础。OSI模型由国际标准化组织（ISO）在1984年提出，旨在为不同厂商生产的设备和系统之间的通信提供一个通用框架。

​       OSI模型将网络通信过程划分为七个独立但相互依赖的层次，每一层都有其特定的功能和协议。通过这种分层结构，复杂的网络通信过程变得更易于管理和理解。

​       从下到上，OSI模型依次分为：物理层、数据链路层、网络层、传输层、会话层、表示层和应用层。

**七层模型释义图示：**

![1718452093275](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452093275.png)

**1.1 物理层（Physical Layer）**

​       物理层是OSI模型的最低层，负责在物理媒体上传输原始比特流。其主要功能包括定义物理设备的硬件规格、传输介质的类型（如电缆、光纤、无线电波）、信号的编码方式和传输速率等。

常见的物理层设备包括集线器（Hub）、中继器（Repeater）和网络适配器（NIC）。

**1.2 数据链路层（Data Link Layer）**

​       数据链路层负责将物理层传输的比特流组装成帧，并提供节点之间的可靠数据传输。主要功能包括成帧、物理地址（MAC地址）管理、错误检测和校正、流量控制和访问控制。

​       常见的数据链路层设备包括交换机（Switch）和网桥（Bridge）。

**1.3 网络层（Network Layer）**

​       网络层负责数据包的路由选择和逻辑地址（IP地址）的处理。其主要功能包括路径选择、逻辑地址管理、分组转发和拥塞控制。

​     常见的网络层设备是路由器（Router）。

**1.4 传输层（Transport Layer）**

​       传输层提供端到端的通信服务，确保数据从发送方到接收方的可靠传输。其主要功能包括端口管理、可靠传输、流量控制和错误检测与校正。

​       传输层的常见协议包括传输控制协议（TCP）和用户数据报协议（UDP）。

**1.5 会话层（Session Layer）**

​       会话层负责管理应用程序之间的会话。其主要功能包括会话建立、管理和终止、会话检查点和恢复、对话控制等。

**1.6 表示层（Presentation Layer）**

​       表示层负责数据的格式化、加密解密和数据压缩。其主要功能是确保发送方和接收方使用一致的数据格式，提供数据的语法和语义转换。

**1.7 应用层（Application Layer）**

​       应用层直接面向用户，提供各种网络服务。其主要功能包括文件传输、电子邮件、远程登录、网页浏览等。



**2. OSI模型每层对应的功能及协议详解**

![1718452109898](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452109898.png)

|                      | 功能                                                         | 常见                            | 协议                                         |
| -------------------- | ------------------------------------------------------------ | ------------------------------- | -------------------------------------------- |
| 物理层(比特Bit)      | 设备间接收或发送比特流；说明电压、线速和线缆等。             | 中继器、网线、集线器、HUB等     | RJ45、CLOCK、IEEE802.3等                     |
| 数据链路层(帧Frame)  | 将比特组合成字节，进而组合成帧；用MAC地址访问介质；错误可以被发现但不能被纠正。 | 网卡、网桥、二层交换机等        | PPP、FR、HDLC、VLAN、MAC等                   |
| 网络层(数据包Packet) | 负责数据包从源到宿的传递和网际互连                           | 路由器、多层交换机、防火墙等    | IP、ICMP、ARP、PARP、OSPF、IPX、RIP、IGRP等  |
| 运输层               | 可靠或不可靠数据传输；数据重传前的错误纠正。                 | 进程、端口（socket）            | TCP、UDP、SPX                                |
| 会话层               | 保证不同应用程序的数据独立；建立、管理和终止会话。           | 服务器验证用户登录、断点续传    | NFS、SQL、NetBIOS、RPC                       |
| 表示层               | 数据表示；加密与解密、数据的压缩与解压缩、图像编码与解码等特殊处理过程 | URL加密、口令加密、图片编解码等 | JPEG、MPEG、ASCII                            |
| 应用层               | 用户接口                                                     | --                              | FTP、DNS、Telnet、SNMP、SMTP、HTTP、WWW、NFS |



**最后：**

​       **给家人们推荐** **常用网络协议学习神图**

![1718452146992](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718452146992.png)

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/oKbGjxX5TPbE002YAYBPkEkRtKVd646EPOaYdlPWEqRUkMN59VAdrqrHorviadyk9MlquNAV9QD223nK6EwVSUw/640?wx_fmt=jpeg&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1)

