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
逻辑：你要从 A 到 B，我给你修一段专用的桥面。只要你在走，别人就绝对不能踩在这块桥面上。物理形态：早期的纵横制交换机（Crossbar Switch）。你会看到机房里密密麻麻的金属杆和触点，拨号时发出“哒哒哒”的机械撞击声。笔记补充：它的限制在于体积。用户越多，交叉点呈 $N^2$ 增长，机房根本放不下。
### 时分交换 (Time Division Switching)
逻辑：它不再物理上一直连接 A 和 B。它把 1 秒钟切成 8000 份（每份叫一个时隙 Time Slot）。

动作：

A 的声音在第 1 个时隙进来。

交换机把这 1/8000 秒的数据存进一个“小盒子”（TSI 时隙交换器）。

当转到 B 的出口时，交换机把盒子打开，把数据放出去。

干货结论：在宏观上，你觉得声音是连续的；但在微观上，你是和几千人共用一根导线，只不过你们每个人只占用了极短的时间碎片。
### 分组交换 (Packet Switching) 