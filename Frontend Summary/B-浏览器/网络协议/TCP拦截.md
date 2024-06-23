# 【网络协议】【TCP】TCP拦截原来是这么实现的！！！

![1719140043555](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719140043555.png)

**3. 三次握手**

​    了解到了TCP标志位的含义，就可以了解TCP的三次握手是怎么进行的了：

![1719140066704](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719140066704.png)

​    发送端发送一个SYN=1，ACK=0标志的数据包给接收端，请求进行连接，这是第一次握手；

​    接收端收到请求并且允许连接的话，就会发送一个SYN=1，ACK=1标志的数据包给发送端，告诉它，可以通讯了，并且让发送端发送一个确认数据包，这是第二次握手；

​    最后，发送端发送一个SYN=0，ACK=1的数据包给接收端，告诉它连接已被确认，这就是第三次握手。之后，一个TCP连接建立，开始通讯。

**3.1 三次握手的过程**

​    根据TCP头部，说明下列3个包在连接建立过程中的次序，为什么？该连接访问的服务是什么服务？

```
0020        00 50 83 aa 46 49 3e dd 33 96 37 a3 a0 12  ...P..FI>.3.7...

0030   16 a0 c4 c0 00 00 02 04 05 b4 04 02 08 0a d7 9b  ................

0040   62 b7 00 56 4a 2a 01 03 03 02                    b..VJ*....   （1）

0020        83 aa 00 50 33 96 37 a2 00 00 00 00 a0 02  .....P3.7.......

0030   16 d0 84 1d 00 00 02 04 05 b4 04 02 08 0a 00 56  ...............V

0040   4a 2a 00 00 00 00 01 03 03 00                    J*........    （2）

0020        83 aa 00 50 33 96 37 a3 46 49 3e de 80 10  .....P3.7.FI>...

0030   16 d0 f3 4b 00 00 01 01 08 0a 00 56 4a 36 d7 9b  ...K.......VJ6..

0040   62 b7                                            b.    （3）
```

解：

在TCP/IP协议中，TCP协议提供可靠的连接服务，采用三次握手建立一个连接。

第一次握手：建立连接时，客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认；

第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。

完成三次握手，客户端与服务器开始传送数据。

（1）是第一次握手，flags位上为02，二进制是0000 0010，即表示有syn没有ack。

（2）是第二次握手，flags位上为12，二进制是0001 0010，即表示有syn和ack。

（3）是第三次握手，flags位上为10，二进制是0001 0000，即表示有ack没有syn。

（4）该连接访问的是80端口，是为HTTP（HyperText Transport Protocol，超文本传输协议）开放的



**3.2 TCP拦截**

​    TCP拦截即TCP intercept，大多数的路由器平台都引用了该功能，其主要作用就是防止SYN泛洪攻击。SYN攻击利用的是TCP的三次握手机制，攻击端利用伪造的IP地址向被攻击端发出请求，而被攻击端发出的响应报文将永远发送不到目的地，那么被攻击端在等待关闭这个连接的过程中消耗了资源，如果有成千上万的这种连接，主机资源将被耗尽，从而达到攻击的目的。我们可以利用路由器的TCP拦截功能，使网络上的主机受到保护(以Cisco路由器为例)。

**开启TCP拦截分为三个步骤：**

\1. 设置TCP拦截的工作模式

​    TCP拦截的工作模式分为拦截和监视。在拦截模式下，路由器审核所有的TCP连接，自身的负担加重，所以我们一般让路由器工作在监视模式，监视TCP连接的时间和数目，超出预定值则关闭连接。

格式：ip tcp intercept mode (intercept|watch)

缺省为intercept

\2. 设置访问表，以开启需要保护的主机

格式：access-list [100-199] [deny|permit] tcp source source-wildcard

destination destination-wildcard

举例：要保护219.148.150.126这台主机

access-list 101 permit tcp any host 219.148.150.126

\3. 开启TCP拦截

ip tcp intercept list access-list-number

示例：我们有两台服务器219.148.150.126和219.148.150.125需要进行保护，可以这样配置：

ip tcp intercept list 101

ip tcp intercept mode watch

........

ip access-list 101 permit tcp any host 219.148.150.125

ip access-list 101 permit tcp any host 219.148.150.126



经过这样的配置后，我们的主机就在一定程度上受到了保护。

**3.3 TCP(Transmission Control Protocol)**

传输控制协议

TCP 是主机对主机层的传输控制协议， 提供可靠的连接服务， 采用三次握手确认建立一个连接:

位码即 tcp 标志位, 有 6 种标示:SYN(synchronous 建立联机) ACK(acknowledgement 确认)

PSH(push 传送) FIN(finish 结束) RST(reset 重置) URG(urgent 紧急)

Sequence number(顺序号码) Acknowledge number(确认号码)



第一次握手：主机 A 发送位码为 syn＝1, 随机产生 seq number=1234567 的数据包到服务器， 主机 B 由 SYN=1 知道， A 要求建立联机；

第二次握手：主机 B 收到请求后要确认联机信息，向 A 发送 ack number=(主机 A 的

seq+1),syn=1,ack=1, 随机产生 seq=7654321 的包

第三次握手：主机 A 收到后检查 ack number 是否正确，即第一次发送的 seq number+1, 以及位码 ack 是否为 1，若正确，主机 A 会再发送 ack number=(主机 B 的 seq+1),ack=1，主机 B 收到后确认 seq 值与 ack=1 则连接建立成功。

完成三次握手，主机 A 与主机 B 开始传送数据。

在 TCP/IP 协议中， TCP 协议提供可靠的连接服务，采用三次握手建立一个连接。



第一次握手：建立连接时，客户端发送 syn 包(syn=j) 到服务器，并进入 SYN_SEND 状态，等待服务器确认；

第二次握手：服务器收到 syn 包，必须确认客户的 SYN（ack=j+1），同时自己也发送一个 SYN包（syn=k），即 SYN+ACK 包，此时服务器 进入 SYN_RECV 状态；第三次握手：客户端收到服务器的 SYN＋ACK 包，向服务器发送确认包 ACK(ack=k+1)，此包发送完毕，客户端和服务器进入 ESTABLISHED 状态，完成三次握手。完成三次握手，客户端与服务器开始传送数据.



**3.3.1 实例**

IP 192.168.1.116.3337 > 192.168.1.123.7788: S 3626544836:3626544836

IP 192.168.1.123.7788 > 192.168.1.116.3337: S 1739326486:1739326486 ack 3626544837

IP 192.168.1.116.3337 > 192.168.1.123.7788: ack 1739326487,ack 1

（1）第一次握手：192.168.1.116 发送位码 syn＝1, 随机产生 seq number=3626544836 的数据包到192.168.1.123,192.168.1.123 由 SYN=1 知道 192.168.1.116 要求建立联机;

（2）第二次握手：192.168.1.123 收到请求后要确认联机信息，向 192.168.1.116 发送 ack

number=3626544837,syn=1,ack=1, 随机产生 seq=1739326486 的包;

（3）第三次握手：192.168.1.116 收到后检查 acknumber 是否正确， 即第一次发送的 seqnumber+1,以及位码 ack 是否为 1，若正确，192.168.1.116 会再发送 ack number=1739326487,ack=1，192.168.1.123 收到后确认 seq=seq+1,ack=1 则连接建立成功。



**3.3.2 图解**

一个三次握手的过程（图 1，图 2）

![1719140147708](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719140147708.png)

（图 1）（上）

![1719140158379](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719140158379.png)

（图 2）（上）

1. 第一次握手的标志位（图 3）

我们可以看到标志位里面只有个同步位，也就是在做请求(SYN)

![1719140171038](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719140171038.png)

（图 3）（上）

1. 第二次握手的标志位（图 4）

我们可以看到标志位里面有个确认位和同步位，也就是在做应答(SYN + ACK)

![1719140185389](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719140185389.png)

（图 4）（上）

1. 第三次握手的标志位（图 5）

我们可以看到标志位里面只有个确认位，也就是再做再次确认(ACK)

![1719140196509](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719140196509.png)

（图 5）（上）

一个完整的三次握手也就是 请求---应答---再次确认



**4. 四次挥手**

​        由于 TCP 连接是全双工的， 因此每个方向都必须单独进行关闭。这个原则是当一方完成它的数据发送任务后就能发送一个 FIN 来终止这个方向的连接。收到一个 FIN 只意味着这一方向上没有数据流动，一个 TCP 连接在收到一个 FIN 后仍能发送数据。首先进行关闭的一方将执行主动关闭，而另一方执行被动关闭。

![1719140214858](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719140214858.png)

（1）客户端 A 发送一个 FIN，用来关闭客户 A 到服务器 B 的数据传送（报文段 4）。

（2）服务器 B 收到这个 FIN，它发回一个 ACK，确认序号为收到的序号加 1（报文段 5）。

​        和 SYN 一样，一个 FIN 将占用一个序号。

（3）服务器 B 关闭与客户端 A 的连接，发送一个 FIN 给客户端 A（报文段 6）。

（4）客户端 A 发回 ACK 报文确认，并将确认序号设置为收到序号加 1（报文段 7）。