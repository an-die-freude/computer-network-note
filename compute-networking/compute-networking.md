## 计算机网络-自顶向下方法

### 1 计算机网络和因特网

#### 1.1 因特网

​		与因特网连接的设备称为**主机**^【因为容纳/运行应用程序】^或**端系统**^【因为位于因特网的边缘】^。

​		主机分为**客户端**和**服务器**。

​		端系统通过**通信链路**和**分组交换机**连接到一起。

​		通信链路的**传输速率**的单位是bit/s。一台端系统向另一台端系统发送数据时，发送端将数据分段并为每段加上首部字节，由此形成的信息包称为**分组**。

​		**路由器**和**链路层交换机**是最著名的分组交换机。链路层交换机通常用于接入网中，而路由器通常用于网络核心中。从发送端系统到接收端系统，一个分组所经历的一系列通信链路和分组交换机称为通过该网络的**路径**。

​		端系统通过**因特网服务提供商**接入因特网。

​		端系统、分组交换机和其他因特网部件都要运行一系列**协议**，这些协议控制因特网中信息的接收和发送。**IP**协议定义了在路由器和端系统之间发送和接收的分组格式。因特网的主要协议统称为**TCP/IP**。

​		**协议**定义了两个或多个通信实体之间交换的报文的格式和顺序，以及发送/接收一条报文或其他事件所采取的动作。

​		**因特网标准**由**因特网工程任务组**研发，其标准文档称为**请求评论**。

​		应用程序涉及多个相互交换数据的端系统称为**分布式应用程序**。

​		与因特网相连的端系统提供了一个**套接字接口**，该接口规定了运行在端系统上的程序请求因特网基础设施向运行在另一个端系统上的特定目的地程序交付数据的方式。

#### 1.2 网络边缘

​		**接入网**指将端系统物理连接到其边缘路由器的网络。**边缘路由器**是端系统到任何其他远程端系统的路径上的第一台路由器。



#### 附录 专业术语

**active optical network terminator(AON)** 主动光纤网络

**cable internet access** 电缆因特网接入

**cable modern** 电缆调制解调器

**cable modern termination system(CMTS)** 电缆调制解调器端接系统

**communication link** 通信链路

**client** 客户端

**data center** 数据中心

**digital subscriber line(DSL)** 数字用户线

**distributed application** 分布式应用程序

**edge router** 边缘路由器

**end system** 端系统

**fiber to the home(FTTH)** 光纤到户

**host** 主机

**hybrid fiber coax(HFC)** 混合光纤同轴

**internet engineering task force(IETF)** 因特网工程任务组

**internet protocol(IP) **网络协议

**internet service provider(ISP)**  因特网服务提供商

**internet standard** 因特网标准

**link-layer switch** 链路层交换机

**optical line terminator(OLT)** 光纤线路端连接器

**optical network terminator(ONT)** 光纤网络端连接器

**packet** 分组

**packet switch** 分组交换机

**passive optical network(PON)** 被动光纤网络

**path** 路径

**protocol** 协议

**request for comment(RFC)** 请求评论

**route** 路径

**router** 路由器

**server** 服务器

**socket interface** 套接字接口

**transmission control protocol(TCP)** 传输控制协议

**transmission rate** 传输速率