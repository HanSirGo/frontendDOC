# 【网络协议】精讲TCP三次握手四次挥手流程！图解超赞超详细！！！

**1.** **TCP建立/断开连接流程详解**

​     相对于SOCKET开发者,TCP创建过程和链接拆除过程是由TCP/IP协议栈自动创建的，因此开发者并不需要控制这个过程，但是对于理解TCP底层运作机制相当有帮助。

**1.1 TCP****三次握手** 

​      所谓三次握手(Three-way Handshake)，是指建立一个TCP连接时，需要客户端和服务器总共发送3个包。 

​       三次握手的目的是连接服务器指定端口，建立TCP连接,并同步连接双方的序列号和确认号并交换 TCP 窗口大小信息.在socket编程中，客户端执行connect()时。将触发三次握手。 

# **TCP建立流程**

**第一次握手**：建立连接时，客户端发送SYN(Seq = J)包到服务器，并进入到syn_sent状态。等待服务器确认。

**第二次握手**：服务器收到SYN包，知道了Client端想建立连接. 它会向客户端发送SYN+ ACK包(ack =J+1, Seq = K)，此时进入syn_recv状态。

**第三次握手**：客户端收到SYN+ ACK包，检查ack是否为J+1, 向服务器发送确认包ACK（ack=K+1）。Server检查ack是否为K+1，如果正确则连接建立成功, 客户端和服务器进入到established状态。完成三次握手。

**整个过程参考下图：**

![1719141374438](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141374438.png)

**细致分析🧐🧐✌️✌️**

- **第一次握手**
       客户端发送一个TCP的SYN标志位置1的包指明客户打算连接的服务器的端口，以及初始序号X,保存在包头的序列号(Sequence Number)字段里。

  ![1719141387779](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141387779.png)

- **第二次握手**
       服务器发回确认包(ACK)应答。即SYN标志位和ACK标志位均为1，同时将确认序号(Acknowledgement Number)设置为客户的ISN加1，即X+1。ISN文末有释义。

  ![1719141401998](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141401998.png)

- **第三次握手**
        客户端再次发送确认包(ACK) SYN标志位为0,ACK标志位为1，并且把服务器发来ACK的序号字段+1，放在确定字段中发送给对方，并且在数据段写ISN的+1。

  ![1719141415364](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141415364.png)

- 

> **SYN攻击（****只需了解****）**
>
> ​     在三次握手过程中，服务器发送SYN-ACK之后，收到客户端的ACK之前的TCP连接称为半连接(half-open connect).此时服务器处于Syn_RECV状态.当收到ACK后，服务器转入ESTABLISHED状态.
>
> ​     Syn攻击就是攻击客户端在短时间内伪造大量不存在的IP地址，向服务器不断地发送syn包，服务器回复确认包，并等待客户的确认，由于源地址是不存在的，服务器需要不断的重发直至超时，这些伪造的SYN包将长时间占用未连接队列，正常的SYN请求被丢弃，目标系统运行缓慢，严重者引起网络堵塞甚至系统瘫痪。Syn攻击是一个典型的DDOS攻击。检测SYN攻击非常的方便，当你在服务器上看到大量的半连接状态时，特别是源IP地址是随机的，基本上可以断定这是一次SYN攻击.在Linux下可以如下命令检测是否被Syn攻击
>
>   netstat -n -p TCP | grep SYN_RECV
>
> ​      一般较新的TCP/IP协议栈都对这一过程进行修正来防范Syn攻击，修改tcp协议实现。主要方法有SynAttackProtect保护机制、SYN cookies技术、增加最大半连接和缩短超时时间等.但是不能完全防范syn攻击。

**1.2 三次握手通俗理解**

![1719141473295](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141473295.png)

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)**1.3 TCP四次挥手**

​     TCP的连接的拆除需要发送四个包，因此称为四次挥手(four-way handshake)。客户端或服务器均可主动发起挥手动作，在socket编程中，任何一方执行close()操作即可产生挥手操作。

**TCP断开流程**

1）**第一次挥手**：客户端进程发出连接释放报文，并停止发送数据。释放数据报文首部，FIN=1，其序列号为seq=u

2）**第二次挥手**：服务器收到连接释放报文，发出确认报文，ACK=1，ack=u+1，并且带上自己的序列号seq=v，此时，服务端就进入了CLOSE-WAIT（关闭等待）状态。

3）**第三次挥手**：客户端收到服务器的确认请求后，此时，客户端就进入FIN-WAIT-2（终止等待2）状态，等待服务器发送连接释放报文（在这之前还需要接受服务器发送的最后的数据）。服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，FIN=1，ACK=1, ack=u+1，由于在半关闭状态，服务器很可能又发送了一些数据，假定此时的序列号为seq=w，此时，服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认。

4）**第四次挥手**：客户端收到服务器的连接释放报文后，必须发出确认，ACK=1，ack=w+1，而自己的序列号是seq=u+1，此时，客户端就进入了TIME-WAIT（时间等待）状态。注意此时TCP连接还没有释放，必须经过2个最长报文段寿命的时间后，才进入CLOSED状态。服务器只要收到了客户端发出的确认，立即进入CLOSED状态。

**整个过程参考下图：**

![1719141494765](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141494765.png)

**1.4 四次挥手通俗理解**

![1719141514605](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141514605.png)

**2.TCP三次握****手及四次挥手抓包图示**

  参见**wireshark抓包**，实测的抓包结果并没有严格按挥手时序。我估计是时间间隔太短造成。

![1719141529385](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141529385.png)

**补充知识点**：  

  **ISN:初始化序列号（initial sequence number）**，是在建立tcp三次连接的时候，存储在序列号位置中的数字的代称。也就是说，告诉对方我将要开始发送的初始化序列号是多少，两边都要发这个ISN，**即tcp三次连接中第一个SYN包和第二个SYN+ACK的包都有这个**,如下图：

![1719141542770](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719141542770.png)

