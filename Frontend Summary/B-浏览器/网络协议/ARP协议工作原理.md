# 【网络协议】精讲ARP协议工作原理！图解超赞超详细！！！

# **首先，我们要搞明白一个问题，为什么需要ARP协议呢？**

​    之前在IP数据报中看到，我们需要在首部填写`源地址`和`目的地址`，这两个地址是`Mac地址`。那什么是Mac地址呢？Mac地址又称物理地址，请注意：不要被 “物理” 二字误导认为物理地址属于物理层范畴，物理地址属于数据链路层范畴。一般在在网卡上。

> **这是由于A设备给B设备发送数据报时，可能不知道B设备的mac地址，所以就需要通过ARP协议去查找。**

![1719144954031](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144954031.png)

- step1:  每台主机都会在自己的ARP缓冲区建立一个ARP列表，以表示IP地址和Mac地址的映射关系。当源主机给目的主机发送数据报，会先检查本地的ARP列表是否存在映射关系，如果有，则直接返回

- step2：如果没有，则向本地网段发送一个ARP请求的广播包，查询改主机对应的Mac地址

- step3：此ARP请求中包含了源主机IP地址，源主机Mac地址，目的主机IP地址。网络中的所有主机收到ARP请求后，会检查包中的IP地址是否和自己的IP地址一致，如果不相同就忽略，如果相同则记录下发送端的Mac地址和IP地址添加到自己ARP列表（如果已经存在则更新），并且给源主机发送一个ARP响应数据报，告诉对方自己是要查找的ARP地址。

- step4: 源主机收到ARP响应后，将得到的目的主机的Mac地址添加到自己的ARP列表中，并利用此信息开始传输数据（如果没有收到ARP响应，表示ARP查询失败

  

  

**1. ARP协议介绍**

##### **1.1 ARP协议功能**

- 将一个已知的IP地址解析成MAC地址
- 重复地址检测，检测地址冲突

**无故ARP**: 当一台设备获取到一个Ip地址时 ，会自动发送一个无故ARP，检测是否有设备已使用了此地址。

**1.2 ARP请求报文**

![1719144969499](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144969499.png)

##### **1.3 ARP工作原理**

![1719144998791](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144998791.png)

1. PC1想发送数据给PC2， 会先检查自己的ARP缓存表，只在终端设备上。

2. 如果发现要查找的MAC地址不在表中，就会发送一个 ARP请求广播，用于发现目的地的MAC地址。ARP请求消息中包括PC1的IP地址和MAC地址以及PC2的 IP地址和目的MAC地址(此时为广播MAC地址FF-FF-FF-FF-FF-FF)

3. 交换机收到广播后做泛洪处理，除PC1外所有主机收到 ARP请求消息，PC2以单播方式发送ARP应答， 并在自 己的ARP表中缓存PC1的IP地址和MAC地址的对应关系， 而其他主机则丢弃这个ARP请求消息。

4. PC1在自己的ARP表中添加PC2的IP地址和MAC地址 的对应关系，以单播方式与PC2通信。

   

**1.4 windows当中如何查看arp缓存表**（静态arp和动态 arp）

- **查看arp缓存表 ：arp -a**

![1719145012391](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719145012391.png)

- 不加IP清除所有 ：arp -d
- 加IP只删除改IP ：arp -d [IP]
- 删除arp静态绑定：arp -s IP MAC

**动态ARP表项老化**：在一段时间内（*默认120s*）如果表项中的ARP映射关系始终没有使用，则会被删除。通过及时删除不活跃表项，从而提升ARP响应效率。

**ARP工作原理（简化）：**

1. PC1发送数据给PC2，查看缓存有没有PC2的MAC地址。

2. PC1发送ARP请求消息（广播）。

3. 所有主机收到ARP请求消息，PC2回复ARP应答（单播），其他主机丢弃。

4. PC1将PC2的MAC地址保存到缓存中，发送数据。

   ![1719145024591](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719145024591.png)