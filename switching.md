# Chapter 1: Switching Logic
我们要解决一个核心矛盾：世界上有成千上万部电话，但我不想给每两部电话之间都拉一根线。
- <b>Core Goal:</b> Minimize physical links via centralized nodes.
- <b>Management:</b> Addressing is hierarchical (National -> Regional -> Local). Numbers are pointers to physical line cards.
- <b>Characteristic:</b> Dedicated path (Circuit-switched). High reliability, low resource efficiency.

## PSTN (Public Switched Telephone Network)
It provides the fundamental addressing and signaling framework that modern Internet protocols eventually inherited.
- <b>Definition:</b> The traditional global infrastructure for wired voice services.
- <b>Mechanism:</b> Circuit Switching (dedicated end-to-end path).

## Star Topology 星型网络
### Star Topology
PSTN 并不是一个巨大的星星，而是无数个星星嵌套在一起的“树状层级”
- <b>Mechanism:</b> All nodes connect to a central switch (PSTN End Office).
- <b>Efficiency</b>: Dramatically reduces the number of physical cables from $O(n^2)$ to $O(n)$.
- <b>Scalability</b>: Allows for hierarchical expansion (stars within stars). This is how PSTN manages millions of users globally.

### Addressing 寻址
在 PSTN 中，寻址不是一次性完成的，而是逐级解析。以电话号码 021-6xxx-xxxx 为例：
- Step 1（本地端局）：它看到 0，意识到这不是本地呼叫，于是把信号送到长途交换机。 
- Step 2（长途交换机）：它看到 21，知道要去上海，于是把信号送到连接上海的中继线（Trunk）。 
- Step 3（上海汇接局）：它看到 6xxx，知道要去某个特定的区，于是分流到对应的端局。 
- Step 4（目标端局）：最后这 4 位数字对应了交换机背面某个物理插槽上的用户线卡（Line Card）。

“寻址”其实就是“查表”。在 PSTN 里，这张表叫路由表；在互联网里，它依然叫路由表

## Switch 交换机如何工作
### 空分交换 (Space Division Switching)
[Video:Panel Telephone Switch](https://www.youtube.com/watch?v=Gsx2ZsYggGw)

[A Crossbar Telephone Switch Explained](https://hackaday.com/2024/02/04/a-crossbar-telephone-switch-explained/)

- 逻辑：你要从 A 到 B，我给你修一段专用的桥面。只要你在走，别人就绝对不能踩在这块桥面上。
- 物理形态：早期的纵横制交换机（Crossbar Switch）。你会看到机房里密密麻麻的金属杆和触点，拨号时发出“哒哒哒”的机械撞击声。
- 工作原理：在每一根横线和纵线的交叉点上，都有一个电子开关。 如果寻址逻辑判断“入口 A”要连“出口 B”，交换机就会合上坐标为 $(A, B)$ 的那个点。
- 笔记补充：只要这个交叉点的开关合上，A 和 B 之间就形成了一条独占的、透明的物理通路。它的限制在于体积。用户越多，交叉点呈 $N^2$ 增长，机房根本放不下。
- 发展史
  - 人工交换时代Manual Switching
  
    一面巨大的塞孔板。当你拿起电话，交换局的接线员（通常是女性）面前的灯会亮起。
    她问：“找哪位？”然后用一根带插头的绳子，一头插在你的孔里，一头插在被叫方的孔里。
    这是最原始的空分交换。接线员的大脑就是最初的“信令处理中心”和“路由表”。
  
  - 步进级交换时代Step-by-Step / Strowger
  
    由拨号脉冲直接驱动机械旋转。
    你拨一个“3”，电流断开3次，交换机里的电磁铁就带动金属臂向上跳3级。
    这是寻址逻辑与物理动作的直接耦合。你拨号的速度就是机器动的速度。

  - 纵横制交换时代Crossbar Switching
  
    它不再是拨一个号动一下，而是先由“记发器（Sender）”把号码全部听完、记下。
    在计算出路径，指挥交叉矩阵上的横杆和纵杆“咔哒”闭合。 
    它实现了控制（Control）与交换（Switching）的初步分离。
  
### 时分交换 (Time Division Switching)
- 逻辑：它不再物理上一直连接 A 和 B。它把 1 秒钟切成 8000 份（每份叫一个时隙 Time Slot）。
- 动作：
  - A 的声音在第 1 个时隙进来。 
  - 交换机把这 1/8000 秒的数据存进一个“小盒子”（TSI 时隙交换器）。 
  - 当转到 B 的出口时，交换机把盒子打开，把数据放出去。
- 干货结论：在宏观上，你觉得声音是连续的；但在微观上，你是和几千人共用一根导线，只不过你们每个人只占用了极短的时间碎片。
- 为什么：比较喜欢哥哥给的解释，就像电影一秒的影像其实只需要十几张图片就可以骗过大脑，声音的记录和再现其实也同样不需要连续的声波。
### 分组交换 (Packet Switching) 

## 编码和解码
graph TD
    A[Voice: Analog Sound] -->|Step 1: PCM| B(Digital Bits: 64kbps)
    B -->|Step 2: TSI| C{Memory Switching}
    C -->|Strategy| D[TDM: Time Division Multiplexing]
    D -->|Result| E[High-Capacity Trunk: E1/T1]
    
    subgraph "The SPC Era (Digital Switching)"
    B
    C
    D
    end

    F[SS7: Signaling System] -.->|Control Plane| C
    style F fill:#f96,stroke:#333,stroke-width:2px