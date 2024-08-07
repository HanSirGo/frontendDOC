# 【网络协议】精讲TCP/IP五层模型，图解超赞超详细！！！

![1719139555690](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719139555690.png)

**1. TCP/IP协议五层模型介绍**

**协议分层模型有：TCP/IP四层模型、TCP/IP五层模型、OSI七层模型。**

![1719139569853](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719139569853.png)

​      作为一名程序员，对于TCP/IP五层协议，重点掌握应用层和传输层，**特别是以下两层对应的协议：**

​    ***1. 应用层：**HTTP协议、FTP协议、SMTP协议和POP3协议等。这些协议是应用程序与网络的接口，程序员需要了解其通信过程和数据格式，从而编写适合的程序进行数据交互。*

​    ***2. 传输层：**TCP和UDP协议。TCP协议可确保传输的数据完整性和顺序性，使用范围较广；UDP协议传输速度更快，但无法保证数据的完整性和顺序性。*

**TCP/IP五层协议模型讲解：**

**应用层：**负责程序之间的沟通，简单的电子邮件传输（SMTP）、文件传输协议（FTP）、网络远程访问协议等（Telent）等。我们程序员网络编程就是针对应用层来进行的。

**传输层：**负责两台主机之间的数据传输。如传输控制协议 (TCP)，能够确保数据可靠的从源主机发送到目标主机。

**网络层：**负责地址管理和路由选择。例如在IP协议中，通过IP地址来标识一台主机，并通过路由表的方式规划出两台主机之间的数据传输的线路（路由）。路由器（Router）工作在网路层。

**数据链路层：**负责设备之间的数据帧的传送和识别。例如网卡设备的驱动、帧同步(就是说从网线上检测到什么信号算作新帧的开始)、冲突检测(如果检测到冲突就自动重发)、数据差错校验等工作。有以太网、令牌环网，无线LAN等标准。交换机（Switch）工作在数据链路层。

**物理层：**负责光/电信号的传递方式。比如现在以太网通用的网线(双绞 线)、早期以太网采用的的同轴电缆(现在主要用于有线电视)、光纤，现在的wifi无线网使用电磁波等都属于物理层的概念。物理层的能力决定了最大传输速率、传输距离、抗干扰性等。集线器（Hub）工作在物理层。

**举例说明：**

​      我在网上买一个物品，需要卖家信息（源IP地址）、我的信息（目的IP地址）。物流（协议）要历经广州，长沙，武汉。运输路径可以是空运（广州直达武汉）、慢达（广州、长沙、武汉）。

*应用层：告诉快递站，卖家要快递给我的货物是什么，根据货物的类型好用相应的包装发送。*

应用层负责程序之间的沟通，规定使用的格式。

*传输层：我和卖家都不关注中间是怎么传输的，只关心起点和终点对应的就是源IP地址与目的IP地址。*

传输层主要关注源IP地址与目的IP地址，不考虑中间路径。

*网络层：发货地址是长沙，收获地址是武汉。长沙到武汉可以空运、火车，网络层可选择合适的路径进行运输。*

网络层主要负责两个遥远节点之间的路径规划。

 *数据链路层：运输路径选择了慢达，广州到长沙使用的是货车，长沙再到武汉使用的火车。*

数据链路层主要负责两个相邻节点之间的传输。

*物理层：网络通信的基础设施，也就是一些网线、光纤、网络接口，也就是网络上的告诉公路。* 



**2. 网络设备所在分层**

![1719139599702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719139599702.png)

​     

​      何为网络设备，就是联网所需要的设备，如电脑主机、路由器、交换机、集线器等。

**主机：**它的操作系统内核实现了从传输层到物理层的内容，**对应的TCP/IP五层模型的下四层即：传输层、网络层、数据链路层、物理层。**

**路由器：**它实现了从网络层到物理层，**对应的是TCP/IP五层模型的下三层即：网络层、数据链路层、物理层。**

**交换机：**它实现从了从数据链路层到物理层，**对应的是TCP/IP五层模型的下两层。**

**集线器：**只实现了物理层。