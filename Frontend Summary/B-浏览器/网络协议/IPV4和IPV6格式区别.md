# 【网络协议】精讲IPV4和IPV6格式区别！图解超赞超详细！！！

### 1. IPV4和IPV6区别

![1719139432076](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719139432076.png)

**1.1 wireshark抓取IPV4包**

![1719139448161](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719139448161.png)

… 0101 = Header Length: 20 bytes (5)  //IP 包头部长度 20bytes

Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT) //差分服务字段

Total Length: 165  //IP 包的总长度

Identification: 0x8a7d (35453)  //标志字段

Flags: 0x0 (Don’t Fragment)  //标记字段

Fragment offset: 0 //碎片偏移量

Time to live: 1  //生存周期 TTL

Protocol: UDP(17) //此包内封装的上层协议为 UDP

Header checksum: 0x0000[validation disabled]  //头部数据的校验和

**Source: 10.130.9.141  //源 IP 地址**

**Destination: 239.192.152.143  //目的 IP 地址**



**1.2 wireshark抓取IPv6包**

![1719139468143](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719139468143.png)

0110......=Version:6  //互联网协议 IPv6

Traffic Class:0x00(DSCP:CS0,ECN:Not-ECT)  //差分服务字段

Flow Label:0x05efb  //流标签

Payload Length:62  //负载长度

Next Header:TCP(6)   //下一个头部所使用的协议类型是TCP

Hop Limit:64   //跳数限制

**Source Address:2001：250：401：6563：d54d:a665:ffea:76da  //源地址**

**Destination Address:2620：1ec：c11::239   //目的地址**



综上：

​    IPv4和IPv6的主要区别体现在地址空间、地址表示法、安全性、自动配置、数据包处理和传输效率等方面。

**地址空间和表示法：**IPv4使用32位地址，可以提供约43亿个唯一的互联网地址，而IPv6使用128位地址，理论上可以提供大约3.4 x 10^38个唯一的互联网地址。IPv4地址由四个十进制数表示，每个数由一个点分隔，例如192.168.1.1。相比之下，IPv6地址由八组四个十六进制数表示，每组数由冒号分隔，例如2001:0db8:85a3:0000:0000:8a2e:0370:7334。

**安全性：**IPv4在安全性方面存在一些问题，如地址伪造、地址欺骗等攻击，其安全性依赖于额外的安全协议（如IPSec）来提高网络安全性。而IPv6在设计时考虑了安全性，并加入了一些安全功能，如IPSec协议的强制性支持、地址随机化、报文认证等机制，提高了网络的安全性。

**自动配置：**IPv4的地址配置通常需要手动进行或通过DHCP（动态主机配置协议）进行，这种方式相对繁琐且存在一定的安全风险。IPv6支持无状态地址自动配置（SLAAC）和DHCPv6等自动配置方式，这些方式可以简化网络配置过程，并降低人工配置错误的风险。

**数据包处理和传输效率：**IPv4的数据包头长度为20个字节，而IPv6的数据包头长度为40个字节。IPv6数据包头部包含了更多有用的信息，如Flow Label字段等，这些字段有助于实现更精细的QoS控制和更高效的数据传输。此外，IPv6还支持更大的数据包大小（通常为1 280个字节），这有助于提高网络传输效率。

​      总的来说，IPv4和IPv6在设计和功能上存在显著差异。随着互联网的快速发展和普及，IPv6的优势逐渐显现出来，尤其是在地址空间、安全性、移动性等方面。虽然目前仍有大量的IPv4网络在运行，但IPv6的应用和部署也在稳步推进中。