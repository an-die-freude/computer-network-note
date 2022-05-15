## 计算机网络-自顶向下方法

### 1 计算机网络和因特网

#### 1.1 因特网

​		与因特网连接的设备称为**主机**^【因为容纳/运行应用程序】^或**端系统**^【因为位于因特网的边缘】^。

​		主机分为**客户端**和**服务器**。

​		端系统彼此交换**报文**。端系统通过**通信链路**和**分组交换机**连接到一起。

​		通信链路的**传输速度**的单位是bit/s。一台端系统向另一台端系统发送报文时，发送端将报文分段并为每段加上首部字节，由此形成的信息包称为**分组**。

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

​		在电路交换的网络中，端系统间通信会话期间，==预留==了端系统间沿路径通信所需要的资源^【缓存和链路传输速度】^，而在分组交换的网络中==不会预留==这些资源。

##### 1.3.1 分组交换

​		多数分组交换机在链路的输入的使用**存储转发传输**机制。存储转发传输是指在交换机能够开始向输出链路传输该分组的第一个比特之前，必须接收到整个分组。

![compute-networking_1](/img/compute-networking_1.png)

​		通过有`N`条速度均为`R`的链路组成的路径，其中有`N-1`台路由器，则端到端时延：$d_{端到端}=N\frac{L}{R}$。

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

​		**传输时延**是路由器推出分组所需的时间，可表示为$\frac{L}{R}$，$L$表示分组的长度，$R(b/s)$表示链路的传输速度，即从队列中退出1bit的速度。		

​		$\frac{L\alpha}{R}$是**流量强度**，其中$\alpha(pkt/s)$表示分组到达队列的平均速度。流量强度主要用于衡量排队时延，==设计系统时流量强度不能大于1==。

​		1bit从一个路由器到另一个路由器所需的时间是**传播时延**。

​		源主机和目的主机之间有$N-1$台路由器，网络通畅^【排队时延可以忽略】^，节点时延累加起来，得到端到端时延：
$$
\begin{align}
d_{end-end}&=N(d_{proc}+d_{trans}+d_{prop})\\&=N(d_{proc}+\frac{L}{R}+d_{prop})
\end{align}
$$

​		**吞吐量**是进程交互比特的速度。

#### 1.5 协议层次及其服务模型

​		某层的**服务模型**是该层向上一层提供的服务。

​		各层的所有协议被称为**协议栈**。

![compute-networking_6](/img/compute-networking_6.png)

##### 1.5.1 因特网协议栈

|        | 功能                                           | 主要协议               | 分组名称 |
| ------ | ---------------------------------------------- | ---------------------- | -------- |
| 应用层 | 存留网络应用程序及它们的应用层协议             | HTTP、SMTP、FTP和DNS等 | 报文     |
| 传输层 | 应用程序端点之间传输应用层报文                 | TCP和UDP               | 报文段   |
| 网络层 | 也称为IP层，将数据报从一台主机移动到另一台主机 | IP                     | 数据报   |
| 链路层 | 沿着路劲将数据包传递给下一个节点               | 以太网、WiFi和DOCSIS   | 帧       |
| 物理层 | 将帧中的一个个比特从一个节点移动到下一个节点   |                        | 比特     |

|      应用      | 应用层协议 |  传输层协议和端口   |
| :------------: | :--------: | :-----------------: |
|    电子邮件    |    SMTP    | TCP:25/465/587/2525 |
|  远程终端访问  |   Telnet   |      TCP:8080       |
|      Web       |    HTTP    |       TCP:80        |
|    文件传输    |    FTP     |      TCP:20/21      |
| 远程文件服务器 |    NFS     |      UDP:2049       |
|   流式多媒体   |  通常专用  |       TCP/UDP       |
|   因特网电话   |  通常专用  |       TCP/UDP       |
|    网络管理    |    SNMP    |     UDP:161/162     |
|    域名系统    |    DNS     |       UDP:53        |
|    SSH隧道     |    SSH     |       TCP:22        |

##### 1.5.2 OSI模型

​		**网络体系结构**是通信系统的整体设计，其广泛采用OSI模型。

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

​		**套接字**是应用程序进程和传输层协议之间的接口。一个进程可以有多个套接字。

​		应用程序开发者可以通过套接字控制应用层的一切，但是对传输层的控制仅限于选择协议和设定几个传输层参数。应用程序体系结构通常使用客户端/服务器体系结构和对等体系结构。

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

​		HTTP服务器不保存关于客户端的任何信息，故HTTP是一个**无状态协议**。为了识别客户端，HTTP使用了cookie。

​		**持续连接**指一个TCP可以传输多个HTTP请求和响应。

​		**非持续连接**指一个TCP只能传输一个HTTP请求/响应对。

​		**Web缓存器**，也称为**代理服务器**，能够代表Web服务器来满足HTTP请求的网络实体。

​		HTTP的**条件GET**机制可以解决Web缓存器缓存的数据陈旧的问题。

​		HTTP主要是一个**拉协议**，TCP连接是由接收端发起。

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

​		SMTP是因特网电子邮件中主要的应用层协议。它限制了邮件的报文(不只是首部)只能采用7-bit ASCII表示，若报文包含非7-bit ASCII字符或二进制数据，需要进行7 bit ASCII编码。

​		SMTP是**推协议**，TCP连接是发送端发起的。

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

##### 2.5.1 P2P体系结构

​		**分发时间**是所有$n$个对等方得到$f(bit)$文件的副本所需时间。

![compute-networking_14](/img/compute-networking_14.png)

​		$D_{cs}$表示C/S体系结构的分发时间，其中服务器需要上传$n$个文件的副本，则
$$
\begin{align}
D_{cs}&\geqslant max\{D_c,D_s\}\\&\geqslant max\{\frac{f}{d_{min}},\frac{nf}{u_s}\}
\end{align}
$$
​		$D_{P2P}$表示P2P体系结构的分发时间，分发开始时只有服务器有文件，分发一次后可由对等方来分发，则
$$
\begin{align}
D_{P2P}&\geqslant max\{D_c,D_s,\frac{nf}{u_{total}}\}\\&\geqslant max\{\frac{f}{d_{min}},\frac{f}{u_s},\frac{nf}{u_s+\sum_{i=1}^{n}u_i}\}
\end{align}
$$

##### 2.5.2 BitTorrent

​		参与文件分发的所有对等方的集合称为**洪流**。在一个洪流中的对等方彼此下载等大小的文件块，通常是256KB。每个洪流具有一个基础设施节点，称为**追踪器**。当有对等方加入洪流时需要向追踪器注册并周期性地通知是否在洪流中。

​		![compute-networking_15](/img/compute-networking_15.png)

​		与对等方建立TCP连接的其他对等方称为该对等方的**邻居/邻近对等方**。

​		**最稀缺优先**即优先请求本身没有且邻居中副本最少的块。这样可以大致地均衡每个块在洪流中的副本量。

​		对等方优先响应那些当前能够以==最高速度==提供副本的邻居。对等方会每十秒进行速度测量并确定速度前四的邻居，这四个邻居会被**疏通**。此外，每三十秒会随机再选择一个邻居进行交换，如果彼此都能满足，则继续交换。除了这五个邻居，其他邻居将被阻塞，即无法从该对等方获取块。这种机制被称为**一报还一报**。

#### 2.6 CDN

##### 2.6.1 视频流

​		一张未压缩/数字编码的图像有像素阵列组成，每个像素由一些比特编码来表示亮度或颜色。

​		视频是一系列图像以每秒二十四/三十张图像来展现。视频能被压缩，故可以比特率来权衡视频质量。

​		在DASH中，视频编码为比特率不同的多个版本，每个版本都有一个不同的URL，每个版本的URL和比特率都存在HTTP服务器中的**告示文件**中。DASH运行客户端自由地切换版本。

##### 2.6.2 CDN

​		**CDN**分布在多个地理位置的服务器上，并且将用户请求重定向到一个时延更低的CDN。

​		CDN分为**专用CDN**和**第三方CDN**，专用CDN由内容提供商自己拥有，第三方CDN分发多个内容提供商的内容。

​		CDN通常采用**深入**和**客邀**这两种安置原则。

​		﹡深入由Akamai首创，该原则通过在遍及全球的接入ISP中部署服务器集群来深入到ISP的接入网中。其目标是靠近端系统，通过减少端系统和CDN集群之间的链路和路由器来减少时延和提供吞吐量，但也带来了较高的维护管理成本。

​		﹡客邀被Limelight和很多其他CDN公司采用，该原则通过在少量的关键位置(通常是因特网交换点)建立大量集群来客邀ISP。相比深入，客邀的维护管理成本更低，但时延较高而且吞吐量较低。

![compute-networking_16](/img/compute-networking_16.png)

​		大多数CDN利用DNS来截获和重定向请求。

​		1）客户端访问某Web网页。

​		2）客户端访问该Web下的某个资源时，发送了对应的DNS请求。

​		3）本地DNS服务器将DNS请求中继到该Web的权威DNS服务器，权威DNS服务器返回了CDN域下的主机名。

​		4）本地DNS服务器通过CDN域下的主机名向CDN权威DNS服务器发送DNS请求，CDN权威DNS服务器返回了CDN服务器的IP地址。

​		5）本地DNS服务器将IP地址返回给客户端。

​		6）客户端通过IP地址与CDN服务器建立TCP连接并发送HTTP请求。

​		CDN部署的核心都是**集群选择策略**，即动态地将请求重定向到CDN中的某个服务器集群/数据中心。

​		较简单的选择策略是将请求重定向到(距离DNS服务器)**地理上最近**的集群。另一种选择策略是对DNS服务器和集群之间进行周期性的时延以及丢包**实时测量**来选择。

### 第三章 传输层

​		网络层提供了主机之间的逻辑通信，传输层提供不同主机的进程之间的逻辑通信。

​		运输层的**多路复用**与**多路分解**指将主机间交付扩展到进程间交换。

​		﹡多路复用指在不同套接字中收集数据块并为每个数据块封装上首部信息从而生成报文段再将报文段传递到网络层。

​		﹡多路分解指将传输层报文段中的数据交付到正确的套接字。

​		多路复用需要**源端口号字段**^【套接字的唯一标识符】^和**目的端口号字段**^【报文段中标识目标套接字的字段】^。

​		端口号是一个16bit的数。0~1023之间的端口称为**周知端口号**。

#### 3.1 UDP

​		UDP在发送报文段前传输层实体间没有握手，故UDP被称为==无连接==的。

![compute-networking_17](/img/compute-networking_17.png)

​		UDP首部有四个字段，每个字段由两个字节构成。

​		﹡长度字段即报文段中的字节数(首部+应用数据)。

​		﹡校验和用于差错检测。

![compute-networking_18](/img/compute-networking_18.png)

​		发送端在计算校验和时需要先加上伪首部并将校验和字段置零，将伪首部、首部和应用数据转换成16位二进制(不足部分填充零)并求和，求和时需要回卷(如果进位到第17位则将结果加一)，将和取反得到校验和，发送端将设置校验和并去掉伪首部。接收端计算校验和方式类似于发送端(不需要将校验和置零)，最后结果全为一则说明数据无误，否则警告。

​		由于无法确保链路的可靠和内存中的差错检测，UDP在端到端基础上的传输层进行差错检测，这种设计被称为**端到端原则**，即同样功能的实现成本在较低级别相比较高级别可能较高。

#### 3.2 可靠数据传输

![compute-networking_19](/img/compute-networking_19.png)

##### 3.2.1 rdt1.0

![compute-networking_20](/img/compute-networking_20.png)

​		rdt1.0协议指经完全可靠信道的可靠数据传输，故接收端就不需要提供任何反馈信息给发送端。

​		FSM的初始状态用虚线表示。发送端和接收端的FSM都只有一个状态，故变迁必定是从一个状态返回到本身。

​		较高层应用调用发送端的过程中，发送端只通过`rdt_send(data)`接收数据，经由`make_pkt(data)`产生一个包含该数据的分组，并将分组发送到信道中。

​		较低层协议调用接收端的过程中，接收端通过`rdt_rcv(packet)`从底层信道接收一个分组，经由`extract(packet,data)`取出数据，并通过`deliver_data(data)`将数据传输给较高层。

##### 3.2.2 rdt2.0

​		通过**肯定确认**或**否定确认**来让发送端知道那些内容被正确接收或接收有误需要重传的可靠传输协议称为**自动重传请求**协议。自动重传请求协议还需要==差错检测==、==接收端反馈==^【用1bit来表示，0是NAK，1是ACK】^和==重传==来处理比特差错的情况。

![compute-networking_21](/img/compute-networking_21.png)

​		rdt2.0相比rdt1.0，加入了差错检测和肯定/否定确认。

​		当发送端等待ACK/NCK时不能从上层获取数据或发送分组，故rdt2.0被称为**停等**协议。

​		发送端有两个状态。在左边的状态中，发送端正在等待上层调用。当出现`rdt_send(data)`时，发送端将通过`make_pkt(data,checksum)`产生一个包含数据和校验和的分组，经由`udt_send(sndpkt)`发送该分组。在右边的状态中，发送端正在等待接收端回传的ACK/NAK。若收到ACK分组，即`rdt_rcv(rcvpkt) && isACK(rcvpkt)`，发送端会回到等待上层调用的状态。若收到NAK分组，即`rdt_rcv(rcvpkt) && isNAK(rcvpkt)`，发送端会重传分组并等待接收回传的ACK/NAK。

​		接收端只有一个状态。当分组到达时，接收端回传ACK/NAK，即`rdt_rcv(rcvpkt) && notcorrupt(rcvpkt)`或`rdt_rcv(rcvpkt) && corrupt(rcvpkt)`。

​		但是rdt2.0忽视了ACK/NAK分组受损的情况，解决这一问题的简单方法就是添加一个新字段来表示发送数据分组的**序列号**。

![compute-networking_22](/img/compute-networking_22.png)

​		rdt2.1是rdt2.0的修订版，rdt2.1的发送端和接收端FSM的状态数都是以前的两倍，因为需要反映出目前分组的序列号。rdt2.1使用了接收端到发送端的ACK/NAK。当收到失序的分组时，接收端回传ACK。当收到受损的分组时，接收端回传NAK。

![compute-networking_23](/img/compute-networking_23.png)

​		rdt2.2相比rdt2.1，rdt2.2无NAK，而是对上一次正确接收的分组回传ACK。发送端收**冗余ACK**后，就知道了接收端没有正确接收到冗余ACK对应的分组后的分组。因此，ACK报文需要一个序列号字段。

##### 3.2.3 rdt3.0

![compute-networking_24](/img/compute-networking_24.png)

​		rdt3.0是用于具有比特差错的丢包信道的协议。通过在发送端中加入**倒数定时器**来解决超时/丢包问题，接收端与rdt2.2相同。

​		因为分组序列号在0和1之间交替，rdt3.0也被称为**比特交替协议**。

##### 3.2.4 流水线可靠数据传输协议

![compute-networking_25](/img/compute-networking_25.png)

​		停等协议存在一定的性能问题，简单的解决方式就算不使用停等，运行发送方发送多个分组而无线等待。因为许多从发送端到接收端的分组可以被看出是填充到一条流水线，故这种技术被称为**流水线**。

​		流水线需要可靠数据传输协议增加序列号的范围和发送/接收端缓存分组，而这些取决于差错恢复。流水线的差错恢复包括**回退N步**和**选择重传**。

##### 3.2.5 回退N步

​		

### 附录1 专业术语

> **active optical network terminator(AON)** 主动光纤网络
>
> **alternating-bit protocol** 比特交替协议
>
> **application programming interface(API)** 应用程序编程接口
>
> **automatic repeat request(ARQ)** 自动重传请求
>
> **average throughput** 平均吞吐量
>
> **band-width** 带宽
>
> **bandwidth-sensitive application** 带宽敏感的应用
>
> **Berkeley Internet Name Domain(BIND/NAMED)** DNS服务器软件
>
> **best-effort delivery service** 尽力而为交付服务
>
> **bidirectional data transfer** 双向/全双工数据传输
>
> **botnet** 僵尸网络
>
> **bottleneck link** 瓶颈链路
>
> **bring home** 客邀
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
> **client** 客户端
>
> **cluster selection strategy** 集群选择策略
>
> **communication link** 通信链路
>
> **congestion control** 拥塞控制
>
> **content distribution network(CDN)** 内容分发网络
>
> **content provider network** 内容提供商网络
>
> **countdown timer** 倒数计时器
>
> **customer** 客户
>
> **data center** 数据中心
>
> **datagram** 数据报
>
> **denial-of-service(DOS)** 拒绝服务
>
> **demultiplexing** 多路分解
>
> **destination port number field** 目的端口号字段
>
> **digital subscriber line(DSL)** 数字用户线
>
> **distributed application** 分布式应用程序
>
> **distribution time** 分发时间
>
> **DNS caching** DNS缓存
>
> **domain name system(DNS)** 域名系统
>
> **duplicate data packet** 冗余数据分组
>
> **dynamic adaptive streaming over HTTP(DASH)** 经HTTP的动态适应流
>
> **edge router** 边缘路由器
>
> **encapsulation** 封装
>
> **end system** 端系统
>
> **end-end principle** 端到端原则
>
> **end-to-end connection** 端到端连接
>
> **enter deep** 深入
>
> **elastic application** 弹性应用
>
> **extend simple mail transfer protocol(ESMTP)** 扩展简单邮件传输协议
>
> **fiber to the home(FTTH)** 光纤到户
>
> **file transfer protocol(FTP)** 文件传输协议
>
> **finite-state machine(FSM)** 有限状态机
>
> **forwarding table** 转发表
>
> **frame** 帧
>
> **frequency-division multiplexing(FDM)** 频分复用
>
> **geographically closest** 地理上最近
>
> **geostationary satellite** 同步卫星
>
> **go-back-n(GBN)** 回退N步
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
> **logic communication** 逻辑通信
>
> **long-term evolution(LTE)** 长期演进
>
> **loss-tolerant application** 容忍丢失的应用
>
> **low-earth orbiting(LEO)** 近地轨道
>
> **mail server aliasing** 邮件服务别名
>
> **manifest file** 告示文件
>
> **malware** 恶意软件
>
> **message** 报文
>
> **multi-home** 多宿
>
> **negative acknowledgment(NAK)** 否定确认
>
> **net file system(NFS)** 网络文件系统
>
> **network architecture** 网络体系结构
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
> **peer-to-peer(P2P)** 点对点
>
> **persistent connection** 持续连接
>
> **pipelining** 流水线
>
> **point of presence(POP)** 存在点
>
> **positive acknowledgment(ACK)** 肯定确认
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
> **rarest first** 最稀缺优先
>
> **real-time measurement** 实时测量
>
> **reliable data transfer(RDT)** 可靠数据传输
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
> **secure shell(SSH)** 安全外壳
>
> **segment** 报文段
>
> **selective repeat(SR)** 选择重传
>
> **server** 服务器
>
> **shared medium** 共享媒体
>
> **silent period** 静默期
>
> **simple mail transfer protocol(SMTP)** 简单邮件传输协议
>
> **simple network management protocol(SNMP)** 简单网络管理协议
>
> **socket** 套接字
>
> **source port number field** 源端口号字段
>
> **source sockets layer** 安全套接字层
>
> **splitter** 分配器
>
> **stateless protocol** 无状态协议
>
> **stop-and-wait** 停等
>
> **store-and-forward transmission** 存储转发传输
>
> **time-division multiplexing(TDM)**  时分复用
>
> **time to live(TTL)** 生存时间
>
> **tit-for-tat** 一报还一报
>
> **top-down approach** 自顶向下方方法
>
> **top-level domain(TLD)** 顶级域
>
> **torrent** 洪流
>
> **total nodal delay** 节点总时延
>
> **tracker** 追踪器
>
> **traffic intensity** 流量强度
>
> **traffic volume** 通信容量
>
> **transmission control protocol(TCP)** 传输控制协议
>
> **transmission delay** 传输时延
>
> **transmission rate** 传输速度
>
> **unchoked** 疏通
>
> **unguided media** 非导引型媒体
>
> **unidirectional data transfer** 单向/半双工数据传输
>
> **unreliable service** 不可靠服务
>
> **unshielded twisted pair(UTP)** 无屏蔽双绞线
>
> **user agent** 用户代理
>
> **user datagram protocol(UDP)** 用户数据报协议
>
> **utilization** 利用率
>
> **well-known port number** 周知端口号

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
> port相关：RFC 1700、RFC 3232
>
> SMTP相关：RFC 821、RFC 1425、RFC 1511、RFC 1521、RFC 1522、RFC 5321 
>
> UDP相关：RFC 768