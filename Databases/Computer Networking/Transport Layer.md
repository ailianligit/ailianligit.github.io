# Transport Layer

- 传输层协议称为**端到端**或**进程到进程**的协议。



## UDP(TCP)/IP数据报

![image-20210620164027292](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620164027292.png)

- IP头部中协议号字段：TCP-6，UDP-17，ICMP-1，IGMP-2

- 伪IP头的组成部分：源IP地址、目的IP地址、协议号和UDP/TCP**（头部）**的长度。

![image-20210620165934918](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620165934918.png)

- 传输层的数据单元（报文）称为**数据段**（Data Segment）。



## User Datagram Protocol(UDP)

- UDP协议提供**无连接**、**不可靠**的**尽力**服务。
- 接收进程每次接收**一个**完整的数据报，如果进程设置的接收缓冲区不够大，收到的数据报将被截断。接收进程收到的数据段数与接收缓冲区大小无关。
- UDP报文组成部分：源端口号、目的端口号、总长度、校验和、UDP数据

![image-20210620170559892](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620170559892.png)

- **总长度：**UDP报文部分的长度
- **校验和：**由伪IP头、UDP头和UDP数据形成。如果发送方把校验和设置为0，接收方会忽略校验和。
- 知名端口（0~1023）：HTTP-80，ftp ctrl-21，ftp data-20，telnet-23，SMTP-25



## Transmission Control Protocol(TCP)

- TCP协议提供**面向连接**的**可靠**的数据传送服务，提供端到端的逻辑管道和**无比特错**的数据传送。TCP协议只能在两个进程之间建立连接，经过互联网可能会发生丢失或错序。
- TCP为**全双工**协议，提供**流控制**机制和**拥塞控制**机制。
- TCP协议提供可靠的**字节流**服务，字节流服务没有消息边界。
- 一个进程可以与其它进程建立两个以上的TCP连接。
- **Maximum Segment Size：**每个数据段的数据部分**（TCP数据）**的最大长度。

![image-20210620171930402](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620171930402.png)

- **序号：**字节流中每个**字节**均被编号。初始序号采用**基于时间**的方案。数据部分的第一个字节的编号为初始序号**加1**。每个数据段的序号采用其数据部分第一个字节的编号。
- **确认号：**期待接收的下一个数据段的编号。只有设置了确认标志（ACK），确认号才有效。
- **校验和：**由伪IP头、TCP头和TCP数据形成。如果发送方把校验和设置为0，接收方会忽略校验和。
- **通知窗口大小：**发送窗口为确认号之后发送方还可以发送的字节的序号范围。接收方用通知窗口大小告知发送方接收窗口的大小，发送方会据此修改发送窗口的大小。
- **紧急指针**：指出带外数据的边界，**不属于字节流**。只有设置了URG标志，紧急指针才有效。

![image-20210620172746607](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620172746607.png)

- **标志：**URG（包含紧急数据）、ACK（确认号有效）、PSH（接收方尽快将缓存的数据交给接收进程）、RST（连接出现错误，通知对方立即中止连接并释放相关资源）、SYN（发起TCP连接，数据段包含**初始序号**）、FIN（向对方表示自己不再发送**数据**）



## TCP三次握手建立连接和四次握手释放连接

![image-20210620174858145](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620174858145.png)

- 服务器收到客户端发来的连接请求后查看是否有进程监听该端口。如果没有，则发RST拒绝；如果有，则发出SYN+ACK包。
- 每一步均采用**超时重传**，多次重发后将放弃。
- 如果半连接请求队列和全连接请求队列满，后到的连接请求将被直接丢弃。客户端此时认为连接已经建立，并不断发送包含ACK的数据段。服务器一般丢弃收到的ACK。客户端第一个数据段重传几次后也会中止该连接。
- 使用三次握手建立连接的原因：若只有一次握手，客户端只要发送了连接请求就认为TCP连接。当服务器不存在或者没打开时，发送数据只会浪费带宽，并且无法获取服务器传来的**初始序号和选项**；若只有两次握手，客户端可以发送大量伪造源地址的连接请求，服务器确认后以为连接已经建立，最后会耗尽资源，进而拒绝所有合法的连接请求，无法提供正常的服务（DoS攻击-带宽消耗攻击）。
- 即使使用三次握手，服务器也可能因为遭遇恶意攻击而瘫痪。黑客可以控制许多主机对一台服务器进行泛洪攻击，发送大量连接请求去请求服务器服务，由于服务器接收不到返回的确认信息，一直处于等待状态，而分配给这次请求的资源却被有被释放。最终服务器的资源被耗尽，无法为合法用户提供服务，从而使服务器瘫痪。（DDoS攻击-系统资源消耗攻击）

![image-20210620175924641](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620175924641.png)

![image-20210620180337601](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620180337601.png)

- 发送FIN数据段的一方说明它不再发送任何数据了，但是可以发送不含数据的ACK包。
- FIN报文超时自动重发，在若干次重发后依然没有收到确认，则发送**RST报文**给对方后强行关闭连接。先发送FIN报文的一方在ACK发送完毕后需要等待**2MSL**(Maximum Segment Lifetime)的时间才完全关闭连接，原因是等待该连接的数据在因特网中消失。
- 进程异常退出时内核会直接发送**RST报文**而不是四次握手来关闭连接。

![image-20210620180458616](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620180458616.png)



## TCP数据段的传输

- TCP协议使用**选择性重传（Selective Repeat）**，但是不使用NAK。
- 只有一个超时定时器，只要有未确认的数据段就要启动。
- **快速重传（Fast Retransmit）：**如果发送方收到**重复3次**同一数据段的ACK，它就认为其后的数据段（由确认号指出） 已经丢失，在超时之前会重传该数据段。
- **延迟确认（Delayed ACK）：**接收方不在收到数据段立即进行确认，而是延迟一段时间再确认。如果这个期间收到多个数据段，则只需要发送一个确认。如果接收方有数据帧要发往发送方，可以使用捎带确认。  
- **选择性确认：**接收方把收到的数据块通过数据段的选项告知发送方，让发送方不重传这些数据块。



## TCP Timer

**Retransmission Timer：**每个连接只针对第一个未确认的数据段启动重传定时器。所有数据段都已确认，则关闭重传定时器。超时重传或发送窗口移动时要重启该定时器。

**Persist Timer：**持续定时器用于保持窗口大小信息流动，即便连接的另一端关闭了接收窗口。

**Keepalive Timer：**保活定时器在长时间没有交换数据段之后，用于检测连接的另一端是否出了问题。  



## TCP超时计算方法

- 初始公式

![image-20210620215348111](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620215348111.png)

- Jacobson算法

![image-20210620215704709](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620215704709.png)

![image-20210620215713140](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620215713140.png)

- Karn算法：在每次重传时直接把RTO加倍直到数据段首次得到确认，并把这个RTO作为后续段的RTO。   

![image-20210620220035190](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620220035190.png)



## TCP Congestion Control

- **Flow control** prevents too many data from being sent to **receiver**.

- **Congestion control** prevents too many data from being sent into **network**.

- 拥塞表现：**丢包**（路由器上缓冲区溢出）和**长延迟**（路由器缓冲区排队）

- 拥塞控制的两大类方法：**端到端**的拥塞控制和**网络辅助**的拥塞控制。

- 拥塞检测：超时或收到3个重复的ACK包。

- rate(发送速率)=SWS/RTT，SWS=min{**CongWin**(发送窗口的流量控制), AdvWin(接收窗口要求的流量控制)}

- TCP改变CongWin的三种机制：加性增乘性减、慢启动、在超时时间后的保守方法

- **AIMD：**每个RTT，CongWin**增加1**，直到丢包；在丢包后**CongWin减半**。

- **Slow Start：**

  （1）初始时， CongWin设为**1**，并设置阈值（threshold），如65535，然后发送一个数据段。每收到一个确认，CongWin**+=MSS**。（*Slow Start*）

  （2）当拥塞发生时，快速重传（*Fast Retransmit*），把当前**CongWin（或SWS）的一半**保存为阈值。**【Tahoe算法】**然后CongWin从**1**开始**【Reno算法】**从**减半的阈值**开始**以MSS为单位**开始增长。

  （3）当 CongWin增长到等于或大于阈值时，CongWin每次**增加1**（*Congestion Avoidance*）。

  （4）拥塞发生后，CongWin回到减半后的阈值处开始增长。（*Fast Recovery*）

![image-20210620224032831](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210620224032831.png)



## Long Fat Pipe

![image-20210621002108568](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210621002108568.png)

- Window Size=AdvWin*2^WinScale
- 序号回绕：使用一般数据段的选项“timestamp”（时间戳）区分TCP长肥管道中的同序号数据段。
- 清空管道：当丢包发生时，由于SWS的限制，管道将会被清空。解决的方法是快速重传和快速恢复。



## Deadlock

![image-20210621004309537](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210621004309537.png)

- 当窗口为0时，如果取空接收缓冲区之后发送的ACK丢失，则会发生死锁。

- 在发送窗口为0之后，发送方如果有数据要发送，则启动坚持定时器（Persist Timer），定期从要发送的数据中取一个字节发送出去（Window Probe），直到收到不为0的通知窗口为止。



## Silly Window Syndrome

### Nagle算法

- 背景：发送进程有很多小批量数据要发送，例如用telnet作为远程终端。

- 算法：

  （1）立即发送一个数据段，即使发送缓冲区只有一个字节；

  （2）只有收到上一个数据段的ACK或者发送缓冲区中数据大小超过MSS，才可以
  发送下一个数据段。

- 对于即时性要求高的地方不适用，如Window方式的鼠标操作。

### Clark算法

- 背景：接收进程频繁取走小批量数据。

- 算法：当接收缓冲区的空闲块大小变得很小时，要等到空闲块大小为接收缓冲区大小的一半或达到MSS时才发送ACK。

