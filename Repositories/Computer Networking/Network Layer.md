# Network Layer

Network Layer: 通过**路由选择**实现在**互连网**的节点之间传送数据包（主机到主机）

## IP Datagram

### Switching

#### Circuit Switching

电路交换通过在网络中连接多条物理电路形成一条通路后传送数据。

每条物理电路可以是一条链路或者一条链路通过FDM或TDM形成的通道。

#### Packet Switching

包交换是采用统计多路复用的方法通过网络传送数据包，有**虚电路**和**数据报**两种方式。

##### Virtual Circuit

采用虚电路方式先建立连接，后传送数据。虚电路有**交换式虚电路**和**永久虚电路**两种。

Switched Virtual Circuit: 每次传送数据前都要建立连接，传送完数据后要释放连接。

Permanent Virtual Circuit: 由管理员建好后一直保持，可以随时传送数据。

##### Datagram

不需要建立连接，交换机根据数据包的**目的地址**转发包。

**因特网**采用**数据报**交换技术。



### Internet Protocol(IP)

IP协议是因特网的网络层协议。

IP协议是**可路由**的，IP**地址是全局地址**，按**层**分配。

IP协议提供尽力服务，即**无连接无确认**的数据报服务。

因特网服务模型不确保带宽、不确保有序、不确保及时、不可靠、无拥塞反馈。

IP协议可以运行在任何网络上。



### IP Datagram

![image-20210510105212809](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078965.png)

![image-20210510105247881](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078967.png)

#### Type of Quality(ToS)

现在使用IP数据报的服务类型字段提供不同的服务被重新定义为区分服务(Differeniated Services)。

#### Time-To-Live(TTL)

IP数据报的生存期用于限制其在因特网上的停留时间，实际限制为经过的路由器数，即**跳数（hop）**。

当收到IP数据报时，路由器或主机会把它的TTL减1，如果减到0时还未到达目的地，则该数据报将会被丢弃，路由器会发送一个**ICMP包**告知源主机。

TTL限制了因特网的直径。

#### Identification & Offset

Maximum Transmission Unit: 物理网络可以运载的最大有效载荷。

如果一个数据报的大小大于要承载它的网络的MTU，路由器需要先对该数据报进行**分段(Fragment)**。

源主机每次发送IP数据报时都会把标识字段加1。

分段时**标识的值保持不变**，并且用偏移量字段指出该片段的数据部分**相对原来数据报数据部分**的偏移量，以8字节为单位。

当目的主机收到该数据报的所有片段时，它会**重组(Reassemble)**为原来的数据报。

DF(don't fragment)为1表示不允许分段，MF(more fragment)为1表示后面还有片段。

分段后，改变的字段有总长度、头部校验、MF和偏移量。



## IP Address

### Space & Structure

48位的MAC地址和32位的IP地址都是**全局分配**的。IP地址空间是**分层**的，是**可路由**的，而MAC地址是由厂商分配的。

IP地址属于**接口**，主机或路由器的每个接口可以配置一个或多个IP地址。

一个IP地址可以划分为网络号(network numbers)和主机号(host identifier)。



### Special IP Address

1.私有IP地址: 192.168.xxx.xxx

2.Loopback: 127.xxx.xxx.xxx(Localhost: 127.0.0.1)

3.广播地址：主机号全为1（网络号全1限于直连网中）

4.多播地址：224.xxx.xxx.xxx



### Classed Network

![image-20210510112034429](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078986.png)

在有类网模型中，可用的A、B、C类网个数（区分网络数和地址数）分别要减2，因为网络号全0和全1不可用。

主机号为全1或全0的地址被保留，不能使用。

解决地址耗尽问题的方案是利用**子网掩码**划分子网(subnet mask)，进一步可以利用变长子网掩码(VLSM)。



### Classless Inter-Domain Routing(CIDR)

无类域间路由选择协议（CIDR）允许把多个有类网合并为一个更大的网络，称为超网（supernet）。

Route Aggregation: CIDR可以显著减少路由表中路由的数量，称为**路由聚合**。



### Network Address Translation(NAT)

网络地址转换是一种把**内部地址映射为外部地址**的技术。

动态NAT: 出口路由器在内网数据报发往外网时自动把内网地址映射为外网地址。TTL内没有使用，则会被删除。

静态NAT: 直接由管理员加入的映射，不会被自动删除。

Network Address Port Translation: 把端口号也加入到NAT的映射中。

Demilitarized Zone(DMZ): 内网主机可以使用内部地址或全局地址访问DMZ的服务器，外部主机只能通过全局地址访问DMZ的服务器，不能访问内网主机。



### Address Resolution Protocol(ARP)

地址解析协议可以把**直连网**中的**IP地址映射为MAC地址**。

ARP协议没有超时重传机制。

分为静态ARP映射和动态ARP映射两类。

当收到ARP请求，**目的主机**会缓存源主机的映射。



### Dynamic Host Configuration Protocol(DHCP)

DHCP协议用于主机在加入网络时**动态租用**IP地址。

DHCP数据报采用UDP分组进行传送，仅限于在直连网内使用。



### Internet Control Message Protocol(ICMP)

因特网控制消息协议用于主机或路由器发布网络级别的**控制消息**。

常见类型有不可达消息、Ping请求和应答消息、超时消息和重定位消息等。



## Routing Protocols

### Routing of Classed Network

Routing Table: 如果目的网络为直连网络，则下一跳(hop)为空。

有类网的路由选择算法：

①利用数据包中的目的地址得到目的网络号，然后查询路由表；

②如果查询的结果为直连网，则直接把数据包从查出的接口转发到目的主机；

③如果查询得到下一跳（路由器），则把数据包转发给下一跳；

④如果没有查到任何匹配项，则把数据包转发给默认路由器，如果没有设置默认路由，则丢弃该数据包。



### Routing of Classless Network

The longest match rule: 当有多条路由都匹配时，选择子网掩码最长的路由。



### Routing Protocols

静态路由：由管理员手工建立。默认路由和直连路由都是静态路由。

动态路由：由路由协议自动建立。



### Autonomous Systems(AS)

自治系统：在同一个机构管理下的网络。

Interior Gateway Protocols(IGP): 内部网关协议用于在AS内建立动态路由，例如RIP协议和OSPF协议。一个AS通常运行单一IGP，也可以运行多个IGP协议，形成多个路由选择域（Routing Domain）。

Exterior Gateway Protocols(EGP): 外部网关协议用于在AS之间建立动态路由。例如BGP协议。



### Routing Algorithm

路由算法：找最短路径（不形成回路）的算法。



## RIP Protocol



## OSPF Protocol



## BGP Protocol

- 最早的外部网关协议（EGP）是EGP协议（Exterior Gateway Protocol），它只允许树形结构的连接。
- 现在主要使用的外部网关协议是边界网关协议（Border Gateway Protocol），它允许**图形**方式的连接。
- BGP协议采用**可靠扩散**（reliable flooding）的方法把AS内的网络的信息传遍整个因特网。
- 全局AS号（1–64511）由ICANN的下属机构进行统一分配。64512-65535为私有AS号。

### 工作原理

- 在AS中，每个运行了BGP协议的路由器被称为**BGP路由器**，所运行的BGP协议被称为**BGP发言人**(BGP Speeker)，而其它路由器称为**内部路由器**（internal router）。
- 在BGP路由器之间可以通过**TCP**连接（端口号为179）建立相邻关系。
- AS内的两个BGP路由器之间建立的相邻关系称为**iBGP**（interior BGP）相邻关系，而位于不同AS的两个BGP路由器之间建立的相邻关系称为**eBGP**（exterior BGP）相邻关系。
- BGP协议所扩散的网络前缀（**网络号**）称为网络层可达信息（Network Layer Reachablility Information）。
- BGP路由器可以把网络层可达信息连同它们的属性一起通过相邻关系扩散给邻居，进而扩散到因特网中所有的BGP路由器。
- BGP路由器在NLRI**引入**BGP协议时扩散一次，并不定期扩散。

### 形成和扩散NLRI

![image-20210702200330940](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078980.png)

- ORIGIN：指出形成NLRI的方法，IGP表示是由管理员加入的，INCOMPLETE表示是聚合形成的。

  NEXT HOP：上一个AS的出口的IP地址。

  AS_PATH：记录经过AS的AS号。  

- 与谁建立相邻关系是由AS管理员指定的。把AS中的哪些网络前缀形成NLRI发布出去一般是由AS管理员指定的，也可以采用**自动产生（重发布）和聚合**产生。

- 这些指定网络只有路由表中**存在**才会被发布该路由的路由器扩散出去。如果它们失效，则会把**撤销**路由的消息扩散出去。

- 为了防止NLRI在AS**之间**扩散时形成**回路**，BGP路由器会**丢弃**所收到的AS_PATH中包含当前AS号的NLRI。

- 为了防止NLRI在AS**内部**扩散时形成**回路**，BGP路由器不会把从**iBGP邻居**收到的NLRI转发给iBGP邻居。

- 如果从多条路径收到同一个NLRI，在默认情况下选择AS-PATH中**AS数最少**的路径。

- BGP路由器根据NLRI的属性NEXT HOP查询**IGP路由表**得到**NEXT HOP**，就可以使用该路由。如果没有查询到匹配项，则丢弃该NLRI。

- 如果设置了**IGP同步**并且NLRI在IGP路由表中没有匹配项，则该NLRI不能转发给eBGP邻居。如果有匹配项，说明内部路由器的路由表存在该路由。

- BGP路由器可以把多个路由**聚合**(aggregate)为一个路由，形成一个新的NLRI，其NLRI的ORIGIN属性要改为**IMCOMPLETE**。

- 如果iBGP邻居之间的路由要经过内部路由器，那么就要给**IGP路由表**中注入AS外的路由，或者通过**隧道技术**连接iBGP邻居。

### 分组类型

- OPEN报文：用于建立相邻关系。
- KEEPALIVE报文：用于判断邻居是否有效。
- UPDATE报文：用于扩散NLRI。
- NOTIFICATION报文：用于报告错误。

### 分组格式

![image-20210702193435663](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078976.png)

- Length：整个报文的长度。不小于19，不大于4096。
- Marker：用于对等方身份认证机制或同步机制。 

### UPDATE分组格式

![image-20210702193647724](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078972.png)

- Unfeasible routes length：撤销路由（Withdrawn routes）的长度。
- Path attributes：AS_PATH、 NEXT_HOP等。
- NLRI的长度可以从总长度和其它长度计算得来。
- NLRI举例：网络66.168.2.0/24作为NLRI的一项（length=24, Prefix=66.168.2）。



## IP Multicasting



## IPv6

