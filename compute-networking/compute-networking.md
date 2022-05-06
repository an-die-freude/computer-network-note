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

#### 1.4 分组交换网

​		当分组从一个节点/主机/路由器沿着这条路劲到后继节点/主机/路由器，该节点在沿途的每个节点经受了几种不同类型的时延，其中最重要的是**节点处理时延**、**排队时延**、**传输时延**和**传播时延**，这些时延的总和是**节点总时延**。若用$d_{proc}$、$d_{queue}$、$d_{trans}$、$d_{prop}$、$d_{nodal}$分别表示处理时延、排队时延、传输时延、传播时延和节点总时延，则$d_{nodal}=d_{proc}+d_{queue}+d_{trans}+d_{prop}$。

![compute-networking_5](/img/compute-networking_5.png)

​		检查分组首部和决定将该分组导向何处所需要的时间是**处理时延**的一部分。

​		在队列中，当分组在链路上等待传输时，需要经受**排队时延**。

​		**传输时延**是路由器推出分组所需的时间，可表示为$\frac{L}{R}$，$L$表示分组的长度，$R(b/s)$表示链路的传输速率，即从队列中退出1bit的速率。		

​		$\frac{L\alpha}{R}$是**流量强度**，其中$\alpha(pkt/s)$表示分组到达队列的平均速率。流量强度主要用于衡量排队时延，==设计系统时流量强度不能大于1==。

​		1bit从一个路由器到另一个路由器所需的时间是**传播时延**。

​		源主机和目的主机之间有$N-1$台路由器，网络通畅^【排队时延可以忽略】^，节点时延累加起来，得到端到端时延：
$$
\begin{align}
d_{end-end}&=N(d_{proc}+d_{trans}+d_{prop})\\&=N(d_{proc}+\frac{L}{R}+d_{prop})
\end{align}
$$

​		**吞吐量**是进程交互比特的速率。

#### 1.5 协议层次及其服务模型

​		某层的**服务模型**是该层向上一层提供的服务。

​		各层的所有协议被称为**协议栈**。

![compute-networking_6](/img/compute-networking_6.png)

##### 1.5.1 因特网协议栈

|        | 功能                                           | 主要协议             | 分组名称 |
| ------ | ---------------------------------------------- | -------------------- | -------- |
| 应用层 | 存留网络应用程序及它们的应用层协议             | HTTP、SMTP、FTP和DNS | 报文     |
| 传输层 | 应用程序端点之间传输应用层报文                 | TCP和UDP             | 报文段   |
| 网络层 | 也称为IP层，将数据报从一台主机移动到另一台主机 | IP                   | 数据报   |
| 链路层 | 沿着路劲将数据包传递给下一个节点               | 以太网、WiFi和DOCSIS | 帧       |
| 物理层 | 将帧中的一个个比特从一个节点移动到下一个节点   |                      | 比特     |

##### 1.5.2 OSI模型

​		相比因特网协议栈，OSI模型多出表示层和会话层。

​		表示层的作用是使通信的应用程序能够解释交换数据的含义。这些服务包括数据压缩、数据加密和数据描述。

​		会话层提供了数据交换的定界和同步功能，包括了建立检查点和恢复方案的方法。

##### 1.5.3 封装

![compute-networking_7](/img/compute-networking_7.png)

​		在每一层，分组包括首部字段和**有效载荷字段**^【通常是上一层的分组】^。

#### 1.6 网络安全

​		**病毒**是一种需要某种形式的用户交互来感染用户设备的恶意软件。

​		**蠕虫**是一种无须任何明显用户交互就能进入设备的恶意软件。

​		**Dos攻击**包括==弱点攻击==^【发送特殊的报文来控制或宕机】^、==带宽洪泛==^【发送大量分组】^和==连接洪泛==^【创建大量TCP连接】^。

​		用来观察执行协议实体之间交换的报文的基本工具被称为**分组嗅探器**。

​		**IP哄骗**指将具有虚假源地址的分组注入因特网。

### 第二章 应用层

#### 2.1 应用层协议原理

​		**套接字**是应用程序进程和传输层协议之间的接口。

​		应用程序开发者可以通过套接字控制应用层的一切，但是对传输层的控制仅限于选择协议和设定几个传输层参数。

​		当进程向另一台主机的进程发送分组时需要定义目的主机的地址和目的主机中接收进程的标识符。		

​		传输层为应用层提供的服务可分为四类：==可靠数据传输==、==吞吐量==、==定时==和==安全性==。

​		**带宽敏感的应用**具有吞吐量要求，而**弹性应用**能够根据可用的带宽。

​		**应用层协议**定义了运行在不同端系统上的应用程序进程如何相互传递报文：

​		﹡交互的报文类型

​		﹡各种报文类型的语法

​		﹡字段的语义

​		﹡确定一个进程何时以及如何发送报文，对报文进行响应的规则

#### 2.2 HTTP

​		Web的应用层协议是**HTTP**。

​		HTTP使用TCP连接的80端口。

​		HTTP服务器不保存关于客户端的任何信息，故HTTP是一个**无状态协议**。为了识别客户端，HTTP使用了cookie。

​		**持续连接**指一个TCP可以传输多个HTTP请求和响应。

​		**非持续连接**指一个TCP只能传输一个HTTP请求/响应对。

​		**Web缓存器**，也称为**代理服务器**，能够代表Web服务器来满足HTTP请求的网络实体。

​		HTTP的**条件GET**机制可以解决Web缓存器缓存的数据陈旧的问题。

​		HTTP主要是一个**拉协议**，TCP连接是由接收方发起。

##### 2.2.1 HTTP请求报文

```http
GET /somedir/page.html HTTP/2
Host: www.test.com
User-Agent: Mozilla/5.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
```

​		HTTP请求报文的第一行是**请求行**。请求行包括方法字段、URL字段和HTTP版本字段。

​		HTTP请求报文第一行之后的行是**首部行**。

![compute-networking_8](/img/compute-networking_8.png)

##### 2.2.2 HTTP响应报文

```http
HTTP/2 200 OK
content-type: text/javascript; charset=utf-8
content-encoding: br
last-modified: Wed, 03 Nov 2021 01:12:45 GMT
server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
```

​		HTTP响应报文的第一行是**状态行**。请求行包括协议版本字段、状态码字段和相应状态字段。

​		HTTP响应报文第一行之后的行是**首部行**。

![compute-networking_9](/img/compute-networking_9.png)

##### 2.2.3 条件GET

​		如果请求报文是GET方法且请求报文的首部行包括`If-modified-since`，该请求报文就是条件GET请求报文。

![compute-networking_10](/img/compute-networking_10.png)

​		第三步中`If-modified-Since`的值等于第二步中`last-modified`的值，这表示Web服务器仅当指定日期之后该对象修改后才发送该对象，假设该对象在指定日期后没被修改，于是第四步中Web服务器向Web缓存器发送的响应报文中状态码为304且没有对象，表示Web缓存器可以转发缓存的该对象的副本。

#### 2.3 电子邮件

​		因特网电子邮件系统由**用户代理**、**邮件服务器**和**简单邮件传输协议**组成。

![compute-networking_11](/img/compute-networking_11.png)

​		1）发件方调用用户代理撰写内容并发送邮件。

​		2）发件方的用户代理把报文发给发件方的邮件服务器，在这里报文被放在报文队列里。

​		3）发件方邮件服务器上的SMTP客户端创建一个到收件方邮件服务器上的SMTP服务器的TCP连接。

​		4）经过初始SMTP握手后，SMTP客户端通过TCP连接发送报文。

​		5）收件方邮件服务器的SMTP服务器接收到报文并将报文投入到收件方的邮箱。

​		6）收件方调用用户代理查看邮件。

##### 2.3.1 SMTP

​		SMTP是因特网电子邮件中主要的应用层协议，使用了TCP连接的25端口、465端口、587端口和2525端口。它限制了邮件的报文(不只是首部)只能采用7-bit ASCII表示，若报文包含非7-bit ASCII字符或二进制数据，需要进行7 bit ASCII编码。

​		SMTP是**推协议**，TCP连接是发送方发起的。

```ini
S: 220 client
C: EHLO server
S: 250 from | 250 PIPELINING | 250 SIZE 12345
C: AUTH XOAUTH2 oauth
S: 235 2.7.0 Accepted
C: MAIL FROM: <client@email.com> 
S: 250 OK
C: RCPT TO: <server@email.com>
S: 250 OK
C: DATA
S: 354 End data with <CR><LF>.<CR><LF>
C: DATA fragment,content
S: 250 OK: queued as.
C: QUIT
S: 221 Bye
```

​		SMTP协议中客户端发送了5条命令：HELO/EHLO^【ESMTP版本】^(HELLO缩写)、MAIL FROM、RCPT TO、DATA以及QUIT。

​		ESMTP相比SMTP，在发送邮件时==需要验证用户账户==。

##### 2.3.2 电子邮件

```ini
From: from@email.com
To: to@email.com
Subject: subject
```

​		邮件报文的首部行包括环境信息，必须包含一个`From`和一个`To`，可能包含`Subject`以及其他可选首部行。

​		在用户代理建立一个到邮件服务器110端口上的TCP连接后，POP3按照3个阶段进行工作：授权、事务处理以及更新。

​		﹡授权阶段需要用户代理以明文形式发送用户名和密码来认证，主要指令有`user <username>`和`pass <password>`。

​		﹡事务处理阶段中用户代理可以取回报文，也可以添加/取消报文删除标记，主要指令有`list`、`retr`和`dele`。

​		﹡更新阶段在用户代理发出`quit`指令并结束会话后，邮件服务器会删除那些被标记的报文。

​		POP3会话期间，邮件服务器会保留一些状态信息，但是在会话中不会携带这些信息。

​		IMAP使用了TCP连接的143端口。相比POP3，IMAP把报文和文件夹联系起来并且提供了创建/修改/删除文件夹和获取报文某些部分的指令。

#### 2.4 DNS

​		主机可以用**主机名**和**IP地址**来进行识别。

​		一台名为a.com的主机，可能还有别名b.com和c.com，此时a.com是**规范主机名**。

​		**域名系统**是一个由分层的**DNS服务器**实现的分布式数据库，一个让主机查询分布式数据库的应用层协议。主要用于将主机名解析为IP地址，也提供**主机别名**、**邮件服务器别名**和**负载分配**服务。

​		DNS使用了UDP连接的53号端口。

##### 2.4.1 DNS工作原理

​		DNS服务器主要分为**根DNS服务器**、**顶级域DNS服务器**、**权威DNS服务器**和**本地DNS服务器**。

​		﹡根DNS服务器提供顶级域DNS服务器的IP地址。

​		﹡每个顶级域^【如com、org、net、edu和gov等】^和国家的顶级域^【如uk、fr、ca和cn等】^都有顶级域DNS服务器(集群)。顶级域DNS服务器提供权威DNS服务器的IP地址。 

​		﹡因特网上具有公共可访问主机的每个组织机构必须提供公共可访问的DNS记录，这些记录将这些主机的名称映射为IP地址。

​		﹡每个ISP都有一个本地DNS服务器/默认名称服务器

![compute-networking_12](/img/compute-networking_12.png)

​		一般情况下，从请求主机到本地DNS服务器的查询是**递归查询**，其余的查询是**迭代查询**。

​		为了降低时延并减少报文数量，**DNS缓存**广泛使用，因此在大部分DNS查询中根DNS服务器都被绕过。

##### 2.4.2 DNS报文

​		**资源记录**提供了主机名到IP地址的映射，格式为`(Name, Value, Type, TTL)`。

​		﹡`Type = A`时，`Name`是主机名，`Value`是主机名对应的IP地址。

​		﹡`Type = NS `时，`Name`是域名，`Value`是能够获取该域名中主机IP地址的权威DNS服务器的主机名。

​		﹡`Type = CNAME `时，`Value`是别名为`Name`的主机的规范主机名。

​		﹡`Type = MX`时，`Value`是别名为`Name`的邮件服务器的规范主机名。

​		对于某个主机名，若DNS服务器是它的权威DNS服务器，则该DNS服务器会有一条包含该主机名的A型记录。若DNS服务器不是它的权威服务器，则该DNS服务器会有一条该主机名所属域名的NS型记录，还会有一条包含该NS型记录中`Value`的A型记录，还可能会有一条包含该主机名的A型记录。

![compute-networking_13](/img/compute-networking_13.png)

​		DNS报文分为查询和应答报文，报文格式相同。

​		﹡在首部区域，第一个字段占16 bit，用于标识该查询，该字段会被复制到应答报文中来匹配请求。第二个字段有若干1 bit的标志位。0/1标识查询/应答报文。若请求的是权威DNS服务器则应答报文会设置[权威]标志位。若客户端在DNS服务器没有资源记录时希望它执行递归查询则会设置[希望递归]标志位。若DNS服务器支持递归查询则会在应答报文设置[递归可用]标志位。剩下四个字段表示后四个区域数据的数量。

​		﹡问题区域包名称字段和类型字段，名称字段是待查询的主机名称，类型字段对应资源记录中的类型字段。

​		﹡应答区域可以包含多条资源记录。		

​		﹡权威区域包含其他权威DNS服务器的资源记录。

​		﹡附加信息区域包含其他有用的资源记录。

#### 2.5 P2P文件分发

​		

### 附录1 专业术语

> **active optical network terminator(AON)** 主动光纤网络
>
> **application programming interface(API)** 应用程序编程接口
>
> **average throughput** 平均吞吐量
>
> **band-width** 带宽
>
> **bandwidth-sensitive application** 带宽敏感的应用
>
> **Berkeley Internet Name Domain(BIND/NAMED)** DNS服务器软件
>
> **botnet** 僵尸网络
>
> **bottleneck link** 瓶颈链路
>
> **cable internet access** 电缆因特网接入
>
> **cable modern** 电缆调制解调器
>
> **cable modern termination system(CMTS)** 电缆调制解调器端接系统
>
> **canonical hostname** 规范主机名
>
> **circuit** 电路
>
> **circuit switching** 电路交换
>
> **communication link** 通信链路
>
> **content distribution network(CDN)** 内容分发网络
>
> **content provider network** 内容提供商网络
>
> **client** 客户端
>
> **customer** 客户
>
> **data center** 数据中心
>
> **datagram** 数据报
>
> **denial-of-service(DOS)** 拒绝服务
>
> **digital subscriber line(DSL)** 数字用户线
>
> **distributed application** 分布式应用程序
>
> **DNS caching** DNS缓存
>
> **domain name system(DNS)** 域名系统
>
> **edge router** 边缘路由器
>
> **encapsulation** 封装
>
> **end system** 端系统
>
> **end-to-end connection** 端到端连接
>
> **elastic application** 弹性应用
>
> **extend simple mail transfer protocol(ESMTP)** 扩展简单邮件传输协议
>
> **fiber to the home(FTTH)** 光纤到户
>
> **forwarding table** 转发表
>
> **frame** 帧
>
> **frequency-division multiplexing(FDM)** 频分复用
>
> **geostationary satellite** 同步卫星
>
> **guided media** 导引型媒体
>
> **header line** 首部行
>
> **host** 主机
>
> **host aliasing** 主机别名
>
> **hostname** 主机名
>
> **hybrid fiber coax(HFC)** 混合光纤同轴
>
> **hyper text transfer protocol(HTTP)** 超文本传输协议
>
> **instantaneous throughput** 瞬时吞吐量
>
> **internet exchange point(IXP)** 因特网交换点
>
> **internet engineering task force(IETF)** 因特网工程任务组
>
> **internet mail access protocol(IMAP)** 因特网邮件访问协议
>
> **internet protocol(IP) **网络协议
>
> **internet service provider(ISP)**  因特网服务提供商
>
> **internet standard** 因特网标准
>
> **IP spoofing** IP哄骗
>
> **link-layer switch** 链路层交换机
>
> **layer** 分层
>
> **load distribution** 负载分配
>
> **long-term evolution(LTE)** 长期演进
>
> **loss-tolerant application** 容忍丢失的应用
>
> **low-earth orbiting(LEO)** 近地轨道
>
> **mail server aliasing** 邮件服务别名
>
> **malware** 恶意软件
>
> **message** 报文
>
> **multi-home** 多宿
>
> **nodal processing delay** 节点处理时延
>
> **non-persistent connection** 非持续连接
>
> **open system interconnection reference model(OSI model)** 开放式系统互联网通信参考模型
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
> **packet sniffer** 分组嗅探器
>
> **packet switch** 分组交换机
>
> **packet switching** 分组交换
>
> **passive optical network(PON)** 被动光纤网络
>
> **path** 路径
>
> **payload field** 有效载荷字段
>
> **peer** 对等
>
> **persistent connection** 持续连接
>
> **point of presence(POP)** 存在点
>
> **post office protocol-version 3(POP3)** 第三版邮局
>
> **propagation delay** 传播时延
>
> **protocol** 协议
>
> **provider** 提供商
>
> **physical medium** 物理媒体
>
> **pull protocol** 拉协议
>
> **push protocol** 推协议
>
> **queuing delay** 排队时延
>
> **reliable data transfer** 可靠数据传输
>
> **request for comment(RFC)** 请求评论
>
> **request line** 请求行
>
> **resource record(RR)** 资源记录
>
> **round-trip time(RTT)** 往返时间
>
> **route** 路径
>
> **router** 路由器
>
> **segment** 报文端
>
> **server** 服务器
>
> **shared medium** 共享媒体
>
> **silent period** 静默期
>
> **simple mail transfer protocol(SMTP)** 简单邮件传输协议
>
> **socket** 套接字
>
> **source sockets layer** 安全套接字层
>
> **splitter** 分配器
>
> **stateless protocol** 无状态协议
>
> **store-and-forward transmission** 存储转发传输
>
> **time-division multiplexing(TDM)**  时分复用
>
> **time to live(TTL)** 生存时间
>
> **top-down approach** 自顶向下方方法
>
> **top-level domain(TLD)** 顶级域
>
> **total nodal delay** 节点总时延
>
> **traffic intensity** 流量强度
>
> **traffic volume** 通信容量
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
>
> **user agent** 用户代理

### 附录2 相关文档

> cookie相关：RFC 6265
>
> DNS相关：RFC 1034、RFC 1035、RFC 2136、RFC 3007
>
> email相关：RFC 5322
>
> HTTP相关：RFC 1945、RFC 2616、RFC 7540
>
> IMAP相关：RFC 3501
>
> POP3相关：RFC 1939
>
> SMTP相关：RFC 821、RFC 1425、RFC 1511、RFC 1521、RFC 1522、RFC 5321 