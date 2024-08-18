# 【网络协议】精讲Wireshark抓包和基本使用！图解超赞超详细！！！

## 前言

​      在网络编程的过程中，经常需要利用抓包工具对开发板发出或接收到的数据包进行抓包分析。wireshark 是一个非常好用的抓包工具，使用 wireshark 工具抓包分析，是学习网络编程必不可少的一项技能。

### 1. Wireshark 开始抓包示例

​    先介绍一个**使用wireshark工具抓取ping命令操作**的示例，让读者可以先上手操作感受一下抓包的具体过程。


1.1 打开wireshark 2.6.1，主界面如下：

![1723968091319](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968091319.png)

# 

1.2 选择对应的网卡，右键，会出现**Start Capture(开始捕获)**，点击即可进行捕获该网络信息，开始抓取网络包

![1723968121073](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968121073.png)

1.3 执行需要抓包的操作，如ping www.baidu.com。



1.4 操作完成后相关数据包就抓取到了。为避免其他无用的数据包影响分析，可以通过在过滤栏设置过滤条件进行数据包列表过滤，获取结果如下。
说明：**ip.addr == 180.101.49.11 and icmp**  表示只显示ICMP协议且源主机IP或者目的主机IP为119.75.217.26的数据包。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)![1723968147496](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968147496.png)
1.5 wireshark抓包完成，就这么简单。关于wireshark过滤条件和如何查看数据包中的详细内容在后期的文章中介绍。



### 2.WireShark抓包界面

![1723968174125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968174125.png)

说明：数据包列表区中不同的协议使用了不同的颜色区分。

协议颜色标识定位在菜单栏**View --> Coloring Rules**。如下所示
![1723968191723](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968191723.png)

### 3. WireShark 主要分为这几个界面

3.1 **Display Filter(显示过滤器)**， 用于设置过滤条件进行数据包列表过滤。菜单路径：Analyze --> Display Filters。

![1723968219074](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968219074.png)

3.2 **Packet List Pane(数据包列表)**， 显示捕获到的数据包，每个数据包包含编号，时间戳，源地址，目标地址，协议，长度，以及数据包信息。不同协议的数据包使用了不同的颜色区分显示。

![1723968243742](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968243742.png)

3.3 **Packet Details Pane(数据包详细信息),** 在数据包列表中选择指定数据包，在数据包详细信息中会显示数据包的所有详细信息内容。**数据包详细信息面板是最重要的，用来查看协议中的每一个字段**。各行信息分别为：
（1）Frame: 物理层的数据帧概况

（2）Ethernet II: 数据链路层以太网帧头部信息

（3）Internet Protocol Version 4: 互联网层IP包头部信息

（4）Transmission Control Protocol: 传输层T的数据段头部信息，此处是TCP

（5）Hypertext Transfer Protocol: 应用层的信息，此处是HTTP协议

![1723968264137](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968264137.png)

### 4. TCP包的具体内容

​     从下图可以看到wireshark捕获到的TCP包中的每个字段。

![1723968294162](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968294162.png)
\4. Dissector Pane(数据包字节区)。

### 5. Wireshark过滤器设置

​      初学者使用wireshark时，将会得到大量的冗余数据包列表，以至于很难找到自己自己抓取的数据包部分。wireshar工具中自带了两种类型的过滤器，学会使用这两种过滤器会帮助我们在大量的数据中迅速找到我们需要的信息。


**（1）抓包过滤器**

​       捕获过滤器的菜单栏路径为Capture --> Capture Filters。**用于在抓取数据包前设置。**

![1723968320442](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968320442.png)

​      如何使用？可以在抓取数据包前设置如下。

![1723968336107](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968336107.png)
      ip host 60.207.246.216 and icmp 表示只捕获主机IP为60.207.246.216的ICMP数据包。获取结果如下：

![1723968356796](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968356796.png)

**（2）显示过滤器**
       显示过滤器是用于在抓取数据包后设置过滤条件进行过滤数据包。通常是在抓取数据包时设置条件相对宽泛，抓取的数据包内容较多时使用显示过滤器设置条件过滤以方便分析。同样上述场景，在捕获时未设置捕获规则直接通过网卡进行抓取所有数据包，如下

![1723968401438](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968401438.png)

​    执行ping www.huawei.com获取的数据包列表如下

![1723968412343](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968412343.png)
       观察上述获取的数据包列表，含有大量的无效数据。这时可以通过设置显示器过滤条件进行提取分析信息。ip.addr == 211.162.2.183 and icmp。并进行过滤。

![1723968434310](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968434310.png)

​     上述介绍了抓包过滤器和显示过滤器的基本使用方法。**在组网不复杂或者流量不大情况下，使用显示器过滤器进行抓包后处理就可以满足我们使用**。

**后期我们将详细介绍wireshark工具的过滤方法，敬请期待哦~**

