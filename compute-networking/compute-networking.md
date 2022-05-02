## 计算机网络-自顶向下方法

### 1 计算机网络和因特网

#### 1.1 因特网

​		与因特网连接的设备称为**主机**^【因为容纳/运行应用程序】^或**端系统**^【因为位于因特网的边缘】^。

​		主机分为**客户端**和**服务器**。

​		端系统彼此交换**报文**。端系统通过**通信链路**和**分组交换机**连接到一起。

​		通信链路的**传输速率**的单位是bit/s。一台端系统向另一台端系统发送报文时，发送端将报文分段并为每段加上首部字节，由此形成的信息包称为**分组**。

​		交换机主要包括**路由器**和**链路层交换机**。链路层交换机通常用于接入网中，而路由器通常用于网络核心中。从发送端系统到接收端系统，一个分组所经历的一系列通信链路和分组交换机称为通过该网络的**路径**。

​		端系统通过**因特网服务提供商**接入因特网。

​		端系统、分组交换机和其他因特网部件都要运行一系列**协议**，这些协议控制因特网中信息的接收和发送。**IP**协议定义了在路由器和端系统之间发送和接收的分组格式。因特网的主要协议统称为**TCP/IP**。

​		**协议**定义了两个或多个通信实体之间交换的报文的格式和顺序，以及发送/接收一条报文或其他事件所采取的动作。

​		**因特网标准**由**因特网工程任务组**研发，其标准文档称为**请求评论**。

​		应用程序涉及多个相互交换数据的端系统称为**分布式应用程序**。

​		与因特网相连的端系统提供了一个**套接字接口**，该接口规定了运行在端系统上的程序请求因特网基础设施向运行在另一个端系统上的特定目的地程序交付数据的方式。

#### 1.2 网络边缘

​		**接入网**指将端系统物理连接到其边缘路由器的网络。**边缘路由器**是端系统到任何其他远程端系统的路径上的第一台路由器。

​		家庭接入：DSL、电缆、FTTH、拨号和卫星

​		企业/家庭接入：以太网和WiFi

​		广域无线接入：3G/4G/5G和LTE

​		**物理媒体**包括**导引型媒体**^【电波沿着固态媒体传播】^和**非导引型媒体**^【电波在空气或外层空间传播】^。

#### 1.3 网络核心

​		通过网络链路和交换机移动数据有两种基本方法：**分组交换**和**电路交换**。

​		在电路交换的网络中，端系统间通信会话期间，==预留==了端系统间沿路径通信所需要的资源^【缓存和链路传输速率】^，而在分组交换的网络中==不会预留==这些资源。

##### 1.3.1 分组交换

​		多数分组交换机在链路的输入的使用**存储转发传输**机制。存储转发传输是指在交换机能够开始向输出链路传输该分组的第一个比特之前，必须接收到整个分组。

![compute-networking_1](/img/compute-networking_1.png)

​		通过有`N`条速率均为`R`的链路组成的路径，其中有`N-1`台路由器，则端到端时延：$d_{端到端}=N\frac{L}{R}$。

​		对于每条相连的链路，分组交换机有一个用来存储路由器准备发往该链路的分组的**输出缓存/队列**。

![compute-networking_2](/img/compute-networking_2.png)

​		分组需要承受输出缓存的**排队时延**。

​		由于缓存空间是有限的，一个到达的分组可能发现该缓存已被其他待传输的分组完全占满，此时刚到达的分组或已经排队的分组其中之一将被丢弃，称为**分组丢失/丢包**。

​		每个端系统都有IP地址，分组的首部中包含了目的地的IP地址。

​		每台路由器具有一个将目的IP地址(一部分)映射成输出链路的**转发表**。

##### 1.3.2 电路交换

​		链路中的电路是通过**频分复用**或**时分复用**来实现的。

​		对于频分复用，链路的频谱由跨越链路创建的所有连接共享。在连接期间链路为每条连接专用一个频段，该频段的宽度称为**带宽**。

​		对于时分复用，时间被划分为固定期间的帧，并且每个帧又被划分为固定数量的时隙。当网络跨越一条链路创建一条连接时，网络在每个帧中为该连接指定一个时隙。

![compute-networking_3](/img/compute-networking_3.png)

##### 1.3.3 网络的网络

​		因为接入ISP向全球传输ISP付费，故接入ISP被认为是**客户**，而全球传输ISP被认为是**提供商**。

​		在任何给定的区域，可能有一个**区域ISP**，每个区域ISP则与**第一层ISP连接**。

​		任何ISP(除了第一层ISP)可以选择**多宿**。

​		位于相同等级结构层次的邻近一对ISP能够**对等**。

​		**因特网交换点**是一个汇合点，多个ISP能够在这里一起对等。

![compute-networking_4](/img/compute-networking_4.png)

#### 1.4 分组交换网中的时延、丢包和吞吐量

##### 1.4.1 分组交换网中的时延

​		当分组从一个节点/主机/路由器沿着这条路劲到后继节点/主机/路由器，该节点在沿途的每个节点经受了几种不同类型的时延，其中最重要的是**节点处理时延**、**排队时延**、**传输时延**和**传播时延**，这些时延的总和是**节点总时延**。

![compute-networking_5](/img/compute-networking_5.png)

​		检查分组首部和决定将该分组导向何处所需要的时间是**处理时延**的一部分。

​		在队列中，当分组在链路上等待传输时，需要经受**排队时延**。

​		**传输时延**是路由器推出分组所需的时间，可表示为$\frac{L}{R}$，$L$表示分组的长度，$R(b/s)$表示链路的传输速率。

​		1bit从一个路由器到另一个路由器所需的时间是**传播时延**。

###### 1.4.1.1 排队时延与丢包

​		

#### 附录 专业术语

> **active optical network terminator(AON)** 主动光纤网络
>
> **band-width** 带宽
>
> **cable internet access** 电缆因特网接入
>
> **cable modern** 电缆调制解调器
>
> **cable modern termination system(CMTS)** 电缆调制解调器端接系统
>
> **circuit** 电路
>
> **circuit switching** 电路交换
>
> **communication link** 通信链路
>
> **content provider network** 内容提供商网络
>
> **client** 客户端
>
> **customer** 客户
>
> **data center** 数据中心
>
> **digital subscriber line(DSL)** 数字用户线
>
> **distributed application** 分布式应用程序
>
> **edge router** 边缘路由器
>
> **end system** 端系统
>
> **end-to-end connection** 端到端连接
>
> **fiber to the home(FTTH)** 光纤到户
>
> **forwarding table** 转发表
>
> **frequency-division multiplexing(FDM)** 频分复用
>
> **geostationary satellite** 同步卫星
>
> **guided media** 导引型媒体
>
> **host** 主机
>
> **hybrid fiber coax(HFC)** 混合光纤同轴
>
> **internet exchange point(IXP)** 因特网交换点
>
> **internet engineering task force(IETF)** 因特网工程任务组
>
> **internet protocol(IP) **网络协议
>
> **internet service provider(ISP)**  因特网服务提供商
>
> **internet standard** 因特网标准
>
> **link-layer switch** 链路层交换机
>
> **long-term evolution(LTE)** 长期演进
>
> **low-earth orbiting(LEO)** 近地轨道
>
> **message** 报文
>
> **multi-home** 多宿
>
> **nodal processing delay** 节点处理时延
>
> **optical Carrier(OC)** 光载波
>
> **optical line terminator(OLT)** 光纤线路端连接器
>
> **optical network terminator(ONT)** 光纤网络端接器
>
> **output buffer** 输出缓存
>
> **output queue** 输出队列
>
> **packet** 分组
>
> **packet loss** 分组丢包
>
> **packet switch** 分组交换机
>
> **packet switching** 分组交换
>
> **passive optical network(PON)** 被动光纤网络
>
> **path** 路径
>
> **peer** 对等
>
> **point of presence(POP)** 存在点
>
> **propagation delay** 传播时延
>
> **protocol** 协议
>
> **provider** 提供商
>
> **physical medium** 物理媒体
>
> **queuing delay** 排队时延
>
> **request for comment(RFC)** 请求评论
>
> **route** 路径
>
> **router** 路由器
>
> **server** 服务器
>
> **shared medium** 共享媒体
>
> **silent period** 静默期
>
> **socket interface** 套接字接口
>
> **splitter** 分配器
>
> **store-and-forward transmission** 存储转发传输
>
> **time-division multiplexing(TDM)**  时分复用
>
> **tier-1 ISP** 第一层ISP
>
> **total nodal delay** 节点总时延
>
> **traffic intensity** 流量强度
>
> **transmission control protocol(TCP)** 传输控制协议
>
> **transmission delay** 传输时延
>
> **transmission rate** 传输速率
>
> **unguided media** 非导引型媒体
>
> **unshielded twisted pair(UTP)** 无屏蔽双绞线