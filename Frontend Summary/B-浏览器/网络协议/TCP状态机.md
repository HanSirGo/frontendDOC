# 【网络协议】精讲TCP状态机！图解超赞超详细！！！

**1. TCP状态机是TCP连接的变化过程** 

​     Tcp在三次握手和四次挥手的过程，就是一个tcp的状态说明，由于tcp是一个面向连接的，可靠的传输，每一次的传输都会经历连接，传输，关闭的过程，无论是哪个方向的传输，必须建立连接才行，在双方通信的过程中，tcp 的状态是不一样的。

​       下面介绍一下，在三次握手和四次挥手的过程中的几种状态，如下图所示，是tcp状态的变化过程：

![1719144406385](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144406385.png)

**TCP的11种状态：**

**CLOSED：**初始时没有任何连接的状态。

**LISTEN：**服务器监听来自客户端的连接请求(SYN包)。

**SYN_SENT：**客户端socket执行CONNECT连接，发送SYN包，之后等待来自服务器的SYN ACK包(服务器的连接请求和对客户端连接请求的确认)。

**SYN_RCVD：**服务端收到客户端的SYN包并发送服务端SYN ACK包，之后等待客户端对连接请求的确认(ACK包)。

**ESTABLISH：**表示连接建立。客户端发送了最后一个ACK包后进入此状态，服务端接收到ACK包后进入此状态。

**FIN_WAIT_1：**终止连接的一方（通常是客户机）发送了FIN包后进入此状态，之后等待对方FIN包。

**CLOSE_WAIT：**（假设服务器）接收到客户机FIN包之后等待关闭的阶段。在接收到对方的FIN包之后，自然是需要立即回复ACK包的，表示已经知道断开请求。但是本方是否立即断开连接（发送FIN包）取决于是否还有数据需要发送给客户端，若还有数据要发送，则在发送FIN包之前均为此状态。

**FIN_WAIT_2：**客户端接收到服务器的ACK包，但并没有立即接收到服务端的FIN包，进入FIN_WAIT_2状态。此时是半连接状态，即有一方要求关闭连接，等待另一方关闭。

**LAST_ACK：**服务端发动最后的FIN包，等待最后的客户端ACK包。

**C****LOSING：**当主动关闭方处于FIN_WAIT_1时，被动关闭方的 FIN 先于之前的自己发送的 ACK 到达，主动关闭方就直接FIN_WAIT_1 -> CLOSING，（其实就相当于同时关闭)，然后迟来的 ACK 到达时，主动关闭方就从CLOSING -> TIME_WAIT。

**TIME_WAI****T：**客户端收到服务端的FIN包，并立即发出ACK包做最后的确认，在此之后的2MSL(两倍的最长报文段寿命)时间称为TIME_WAIT状态



**2.** **正常状态变迁**

**文末有神图![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)**

**服务端正常状态变迁：**

\1. 服务器通过listen()系统调用后进入LISTEN状态，被动等待客户端连接(被动打开)。

\2. 服务器一旦监听到某个连接请求(收到同步报文段SYN)，就将该连接放入内核请求等待队列中，并向客户端发送SYN ACK同步确认报文段，此时服务端处于SYN_RCVD状态，等待客户端的确认。

\3. 服务器成功接收到客户端发送回的ACK确认报文段后，进入ESTABLISHED状态。(此时已建立连接，双方能够进行双向数据传输)

\4. 数据传输完毕，服务器收到客户端发送的FIN结束报文段后，给客户端发送回ACK确认报文段(服务器可能还有数据要发)，此时服务端处于CLOSE_WAIT状态，服务器继续发送未发完的数据。

\5. 服务器在发送完数据后给客户端发送FIN结束报文段，之后进入LAST_ACK状态，等待客户端对结束报文段的最后一次确认。

\6. 服务器成功收到客户端发送回的ACK确认报文段后，连接彻底关闭，进入CLOSE状态。

**客户端正常状态变迁：**

\1. 客户端通过connect()系统调用给服务器发送一个SYN同步报文段请求建立连接，此时客户端进入SYN_SENT状态。

\2. 客户端成功收到服务器发送回的SYN ACK同步确认报文段后，再给服务器发送一个ACK确认报文段，之后进入ESTABLISHED状态。(此时已建立连接，双方能够进行双向数据传输)

\3. 数据传输完毕后，客户端给服务器发送一个FIN结束报文段，之后进入FIN_WAIT_1状态，等待服务端的确认。

\4. 客户端收到服务器发送的ACK确认报文段后，进入FIN_WAIT_2状态，之后接收服务器发送的还未发完的数据。

\5. 服务器发完数据后，客户端收到服务器发送的FIN结束报文段，同时给服务端发回一个ACK确认报文段，之后进入TIME_WAIT状态。

\6. TIME_WAIT状态下等待2MSL时长内，如果客户端没有收到服务器重传的FIN结束报文段，则连接彻底关闭，此时进入CLOSE状态。

**下图是时序图**

![1719144446296](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144446296.png)

#### 特殊状态变迁

**1. SYN_SENT状态——>CLOSE状态**

​     connect连接的目标端口不存在(未被任何进程监听)，或者该端口仍被处于TIME_WAIT状态的连接所占用，则服务器给客户端发送RST复位报文段，重新请求连接，但如果最终没能成功，则因超时而导致connect调用失败。

​     如果目标端口存在，但connect在超时时间内未收到服务器的确认报文段，则connect调用失败。

**2. FIN_WAIT_1状态——>TIME_WAIT状态**

​     处于FIN_WAIT_1状态的客户端直接收到 ACK FIN确认结束报文段(而不是先收到ACK确认报文段，再收到结束报文段，说明服务端在客户端请求结束连接时没有消息要发)。

**3. LISTEN状态——>SYN_SENT状态**

​     服务器主动向客户端发起连接。

**4. SYN_RCVD状态——>LISTEN状态**

​     服务器如果收到RST报文段，说明客户端请求重新建立连接，所以服务端返回LISTEN状态。

**5. SYN_RCVD状态——>FIN_WAIT_1状态**

​     服务器是接收到客户端发送SYN同步报文段后，向客户端发送回SYN ACK同步确认报文段后进入SYN_RCVD状态，之后再发送FIN结束报文段。

​     正确情况下客户端先收到SYN ACK同步确认报文段，同时发送一个ACK确认报文段给服务器，客户端之后就进入ESTABLISHED状态。之后才会收到FIN结束报文段，客户端是已经建立连接的状态，并且服务端也将会因收到ACK确认报文段而进入ESTABLISHED状态。

​     所以如果处于SYN_RCVD状态的服务端想要结束连接，则需要发送FIN结束报文段来按照正常流程解除连接。因此，SYN_RCVD服务器主动发送FIN结束报文段，进入FIN_WAIT_1状态。

**6. SYN_SENT状态——>SYN_RCVD状态 (同时打开连接)**

​     客户端向服务器发送完SYN同步报文段后进入SYN_SENT状态，等待服务器发送的确认，但是却收到服务器发送的SYN同步报文段。说明双方在接收到对方的SYN同步报文段之前，都进行了主动发起请求连接的操作，即向对方发出SYN同步报文段。

​     所以对方都会先收到SYN同步报文段而不是预想的ACK确认报文段，因此都进入SYN_RCVD状态。

![1719144526587](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144526587.png)

​     两端的状态变化都是由 CLOSED——>SYN_SENT——>SYN_RCVD——>ESTABLISHED。

**7. FIN_WAIT_1状态——>CLOSING状态 (同时关闭连接)**

​     客户端向服务器发送完FIN结束报文段后进入FIN_WAIT_1状态，等待服务发送的确认，但是却收到服务器发送的FIN结束报文段。说明双方在接收到对方的FIN结束报文段之前，都进行了主动发起结束连接的操作，即向对方发出FIN结束报文段。

​     所以对方都会先收到FIN结束报文段而不是预想的ACK确认报文段，因此都进入CLOSING状态。

![1719144538458](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144538458.png)

​     两端的状态变化都是由 ESTABLISHED——>FIN_WAIT——1->CLOSING——>TIME_WAIT——>CLOSED。