# Chapter 1: Switching Logic
我们要解决一个核心矛盾：世界上有成千上万部电话，但我不想给每两部电话之间都拉一根线。
- **Core Goal**: Minimize physical links via centralized nodes.
- **Management**: Addressing is hierarchical (National -> Regional -> Local). Numbers are pointers to physical line cards.
- **Characteristic**: Dedicated path (Circuit-switched). High reliability, low resource efficiency.

## PSTN (Public Switched Telephone Network)
于是我们发明了一种结构，它无需给每个用户之间建立独立通道。而是通过信号管理控制一个庞大的网络，让有限的“路”在节点之间“移动”，只给需要通话的人临时搭一条通道。

It provides the fundamental addressing and signaling framework that modern Internet protocols eventually inherited.
- **Definition**: The traditional global infrastructure for wired voice services.
- **Mechanism**: Circuit Switching (dedicated end-to-end path).

## Topology 通信网络结构
网络拓扑结构是计算机网络结点和通信链路所组成的几何形状。\
有总线、环状、星状、树状、网状等。其中总线、环状、星状是3种基本的拓扑结构。

### 总线结构
网络中结点均通过相应的接口直接连接到称为总线的通信线路上，各结点共享一条数据通道。
- 特点：结构简单灵活、易于实现扩展、共享能力强，网络响应速度快。
- 缺点：传输能力低，安全性较差，总线的故障会导致网络瘫痪。它是局域网常采用的拓扑结构。

### 环状结构
网络中各结点通过接口连在一条首尾相连的闭合环状通信线路中，信息在环路中沿着一个方向在各个结点间传输，通信线路共享。\
分为单环结构(e.g.Token Ring)双环结构(e.g.FDDI)。
- 特点：结构简单，易实现。
- 缺点：可扩充性差，可靠性低。一个结点或一处链路故障，将会造成全网瘫痪。

### 星状结构
以一台网络通信设备为中心结点，其他外围结点通过一条单独的链路与中心结点相连，信息的传输是通过中心结点的存储转发技术实现。在局域网中最常用到。
- 特点：结构简单，易实现、易扩充且便于管理和维护，某外围结点链路故障不影响其他结点正常工作。
- 缺点：对中心结点的要求较高，中心结点负担重，易成为信息传输的瓶颈和单点故障点。

### PSTN结构
并不是一个巨大的星星，而是无数个星星嵌套在一起的“树状层级”
- **Mechanism**: All nodes connect to a central switch (PSTN End Office).
- **Efficiency**: Dramatically reduces the number of physical cables from $O(n^2)$ to $O(n)$.
- **Scalability**: Allows for hierarchical expansion (stars within stars). This is how PSTN manages millions of users globally.

## Addressing 寻址
A digit-by-digit routing process. It prunes the network tree to find the destination port. \
在 PSTN 中，寻址不是一次性完成的，而是逐级解析。以电话号码 021-6xxx-xxxx 为例：
- Step 1（本地端局）：它看到 0，意识到这不是本地呼叫，于是把信号送到长途交换机。 
- Step 2（长途交换机）：它看到 21，知道要去上海，于是把信号送到连接上海的中继线（Trunk）。 
- Step 3（上海汇接局）：它看到 6xxx，知道要去某个特定的区，于是分流到对应的端局。 
- Step 4（目标端局）：最后这 4 位数字对应了交换机背面某个物理插槽上的用户线卡（Line Card）。

“寻址”其实就是“查表”。在 PSTN 里，这张表叫路由表；在互联网里，它依然叫路由表

## Switching System / Exchange
这个网络里的节点——交换机/中继站，是怎么工作的

### Evolution of Switching Systems
📽️️[西雅图的Connections Museum介绍](https://youtu.be/D_m4K5Q5yRU?si=GE8wJw6AUbprgkzN)
▶️[博物馆自己的频道](https://www.youtube.com/c/ConnectionsMuseum)\
可以参观（也许体验）最早的人工接线时代交换机，一直到也许最现代的一台？

📃[电话交换百年史_交换机](links/电话交换百年史_交换机.mhtml)

#### 人工交换时代 Manual Switching
📽️️[AT&T的Switchboards发展史视频](https://www.youtube.com/watch?v=xJ1fKFqt7qU)

一面巨大的塞孔板。当你拿起电话，交换局的接线员（通常是女性）面前的灯会亮起。
她问：“找哪位？”然后用一根带插头的绳子，一头插在你的孔里，一头插在被叫方的孔里。
这是最原始的空分交换。接线员的大脑就是最初的“信令处理中心”和“路由表”。
  
#### 电子机械-步进级交换时代 Step-by-Step / Strowger
📽️[Step-By-Step Telephone Swith](https://youtu.be/VN_K2PgMYq8)

由拨号脉冲直接驱动机械旋转。
你拨一个“3”，电流断开3次，交换机里的电磁铁就带动金属臂向上跳3级。
这是寻址逻辑与物理动作的直接耦合。你拨号的速度就是机器动的速度。

#### 电子机械-纵横制交换时代 Crossbar Switching
它不再是拨一个号动一下，而是先由“记发器（Sender）”把号码全部听完、记下。
在计算出路径，指挥交叉矩阵上的横杆和纵杆“咔哒”闭合。 
它实现了控制（Control）与交换（Switching）的初步分离。

#### 程控交换时代 Stored Program Control, SPC
用计算机软件控制交换动作，进入“时分交换”。 
交换机变成一台运行程序的计算机。声音变成了内存里的比特流，交换动作变成了 CPU 操作内存地址。
容量从几千线瞬间飙升到几十万线。因为有软件，才有了缩位拨号、呼叫转移等功能。

#### 软交换与 IMS Softswitch / VoIP\
现状：交换机不再是一个大柜子，而是运行在通用服务器上的软件。
所有的语音都变成了 IP 包（第三章的内容）。

#### Evolution Summary:
- Manual: Human intelligence + physical cords. 
- Step-by-Step: Pulse drive + mechanical selectors (Strowger). 
- Crossbar: Common control + coordinate matrix (Space Division). 
- SPC (Digital): Computer software + TSI/PCM (Time Division). 
- Softswitch: All-IP network + Cloud software.

Key Insight: The history of switching is a journey of "Dematerialization" — from heavy metal bars to invisible software bits.

### Switch Logic 交换的原理
#### Space Division Switching 空分交换
📽️[Video:Panel Telephone Switch](https://www.youtube.com/watch?v=Gsx2ZsYggGw)\
📽️[Step-By-Step Telephone Swith](https://youtu.be/VN_K2PgMYq8)\
📽️[A Crossbar Telephone Switch Explained](https://hackaday.com/2024/02/04/a-crossbar-telephone-switch-explained/)
- 逻辑：你要从 A 到 B，我给你修一段专用的桥面。只要你在走，别人就绝对不能踩在这块桥面上。
- 物理形态：早期的纵横制交换机（Crossbar Switch）。你会看到机房里密密麻麻的金属杆和触点，拨号时发出“哒哒哒”的机械撞击声。
- 工作原理：在每一根横线和纵线的交叉点上，都有一个电子开关。 如果寻址逻辑判断“入口 A”要连“出口 B”，交换机就会合上坐标为 $(A, B)$ 的那个点。
- 笔记补充：只要这个交叉点的开关合上，A 和 B 之间就形成了一条独占的、透明的物理通路。它的限制在于体积。用户越多，交叉点呈 $N^2$ 增长，机房根本放不下。
  
#### Time Division Switching 时分交换
- 前提：声音/信号的数字化 
- 这里需要知道声音是波，以及对声波采样编码还原的概念
  - PCM 脉冲编码调制 Pulse Code Modulation
    - Pulse 脉冲\
      离散的脉冲信号，不再是连续变化的模拟电压，每个脉冲代表一个数值 
      - 对应：采样之后的结果 
    - Code 编码\
      把采样得到的幅度值用二进制表示（8位二进制码就是 8 bit）
      - 对应：量化 + 二进制表示 
    - Modulation 调制\
      把这些二进制数字变成可以在物理线路上传输的信号形式（电信号/光信号）  
      - 对应：送上线路
    - 例：8 bit 8000次/秒 = 8*8000 = 64k电话 = 电话语音的PCM标准G.711
- 在知道了声音采集是每隔一段时间采集一次之后：
  - 如果这个间隔是125微秒一次，每次采集出来的数据是8bit（1个PCM采样值）
  - 如果一条传输线路，每125微妙能传输32*8bit数据（例：E1线路，线路速率2.048Mbps）
  - 那么理论上这条线路可以同时让32个用户的声音数据一起被传送。(实际只有30路电话，另外2个用于同步和信令)
  - 以上叫做**时分复用** TDM（更严谨地说是同步TDM，区别于异步TDM等）
- 如何把声音正确的送给接电话的人：
  - 32个人的声音数据，每次采集都按同样的顺序依次排列传输，1-32号位置
  - 给这32个打电话的人和他们的声音数据建立好关系表：| 打电话号 | 传输位置 |
  - 接电话的人和打电话的人也有一个对应关系表：| 打电话号 | 接电话号 |
  - 接电话>>打电话>>传输位置，建立取出数据的关系表：| 接电话号 | 传输位置 |
  - 接电话的人每次都从线路的这个位置取出数据
  - 完成以上工作的部件叫**时隙交换器** TSI
  - 延迟：一个时分交换器带来的延迟，理论上最坏是1个周期125微秒0.1毫秒，平均是0.5个周期62.5微秒。
  - 参考：人耳对小于150毫秒的声音延迟几乎无感。对大于400毫秒的感觉无法进行对话。

#### TST结构
手工交换和电子机械交换时代，交换机可能全是空分结构的。但是程控交换时代的交换机，则是一种时分与空分组合起来的结构。

- T时分，S空分，T时分
- Why?
  - 降低交叉矩阵规模：两级时分+空分矩阵只需 $(N/2) × (N/2)$ 规模
  - 延迟可控：2级时分交换延迟仅为*2，空分延迟几乎为零
  - 可扩展：多个 TST 级联即可服务数万路用户
  - 全空分矩阵规模会爆炸，成本极高
  - 全时分一条线可能跨很多输入输出，延迟和复杂度过高，无法实现
- [现代交换原理CH2 TST网络](https://blog.csdn.net/grin99/article/details/104948967)
- When?

  | 交换机类型 | TST 结构 | 用户容量 |
  |------------|----------|----------|
  | 小型本地交换机 | 单级 TST | 几千用户 |
  | 中型城市交换机 | 单级/双级 TST | 10,000–50,000 用户 |
  | 区域/长途交换机 | 多级 TST 串联 | 数十万–百万用户 |
    >注：现代网络大部分已被 分组交换 / VoIP 替代，但 PSTN 时代，这就是规模极限设计方法。

#### Packet Switching 分组交换
和电脑网络一起展开

## 信号、传输、噪声与失真
这部分太偏数学，以后再详细学。我可能比较感兴趣的问题：
- 声音是如何变成电信号的
- 信号如何远距离传输
- 把信号还原成声音会遇到什么问题
- 如何处理噪声与失真
