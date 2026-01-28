# Chapter 2: Control Logic & Signaling
如何让庞而大复杂的电话网络，知道“张三现在要连李四”，并准确无误地给他们俩建立一条通路？

## History
📽️[一个技术宅发的老电话系列视频(英文)](https://youtube.com/playlist?list=PLy4HJ7g3Ybft4ANU15k1Z7dSj6cdOiA12&si=qK4r3tOjhNyUMcil)\
👉[TSSN电信交换系统教程网站](https://www.w3ccoo.com/telecommunication_switching_systems_and_networks/index.html)\
👆[TSSN原始英文教程](https://www.tutorialspoint.com/telecommunication_switching_systems_and_networks/index.htm)

1. 第一代：步进式寻址系统 / 信令同步系统 Step-by-step switching / Direct control system \
   The first ever automatic telephone switching was developed by Almon B Strowger. \
   The Strowger Switching system is also called the step-by-step switching system \
   as the connections are established in a step-by-step manner.\
   这是最原始的一种信号控制：直接利用物理线路的通断，来传递指令：
   - 你转动拨号盘（Rotary Dial）到“3”，会让线路断开并闭合3次。于是能同步驱动交换机的某个元器件转动3次，连上需要接通的线路。
   
   关键结构——棘轮和电磁继电器： 
   - 每次断电的瞬间，磁铁的磁力消失，有个弹簧会拉着棘轮转动一格，拨片移动一个位置且通电磁力回复后也能停住。
   - 步进式交换机：横向运动处理具体数字，纵向运动对应现在是第几个号码。\
     脉冲信号控制电磁继电器，让每一排拨片移动n次；\
     每处理完一个拨号，机器自己往下一层挪动一次，准备处理下一个数字的脉冲信号。
   - 用此方法自动寻找到需要接通的线路。通话结束接收到挂断信号时有一个复位操作。
   
   指令和通话用一条物理线路实现：
   - 拨号的时候，你听到的“嗒嗒嗒”声就是指令。
   - 速度极慢，且只能传简单的数字，没法传“呼叫转移”或“来电显示”等复杂指令。
   - 在旋转拨号盘上拨打一个7位数字的电话号码需要12秒。这套电磁机械控制的交换系统，如果每秒收到超过10-12个的脉冲信号，就会出错（棘轮转不过来，连到错误的线路）
   
2. 第二代：双音多频 DTMF 和通用控制系统 Common Control Subsystem\
   随着用户数量的增加，电话网络变得庞大，进而发展出星状多级连接系统。让每个用户了解应该拨通哪一个中继站exchange才能完成呼叫工作是不现实的。\
   通用控制子系统被引入：
   - 由机器记住电话网络结构，
   - 机器之间直接沟通
   - 找到理想连接线路
   - 给接打电话的人建立通路
   - 同时还能承担计费等其他工作\
   
   这个时候的信令控制也随之升级成了可以支持这种通用控制系统的方式。\
   收到信号和处理连接两个事情被彻底分开，也就不再需要拨号码的人控制发出脉冲的速率。\
   拨盘电话可以升级成按键电话。
   
   按键电话发出的信号是频率信号：
   - 双音多频 (DTMF) 键盘和对应的信号频率
   
     |        | 1209Hz | 1336Hz | 1477Hz |
     |-------:|:------:|:------:|:------:|
     |  697Hz |   1    |   2    |   3    |
     |  770Hz |   4    |   5    |   6    |
     |  852Hz |   7    |   8    |   9    |    
     |  941Hz |   *    |   0    |   #    |   

   - 每个按键被按下时，同时送出一高一低两个频率信号。
   - 信号频段不容易被人声和音乐模仿
   - 高低频各个信号之间的差异比较明显，容易被识别处理
   - 每次发出的信号持续100ms，不容易被丢失或错误记录
   
   信令系统的功能已经开始形成：
   - 

   这依然是带内信令（In-band）：
   - 指令还是跑在语音通道里，安全性差。
   - 当年的黑客可以用特制的口哨模拟频率，实现免费拨打长途（著名的“蓝盒子”事件）。

3. 第三代：随路信令（CAS - Channel Associated Signaling）
   进入程控交换早期，指令开始变得有组织：
   - 做法：在数字中继线（如 E1）中，虽然指令和语音分开了，但依然有很强的绑定关系。
   - 局限：就像是给每一节车厢（时隙）配了一个专门的“信封”，信封贴在车厢外面。这种方式效率低，而且一旦线路繁忙，指令就会延迟。
4. 第四代：SS7（Common Channel Signaling） —— “专用指挥网”这就是你现在正在学的 SS7（7号信令）。
   它是信令史上的巅峰，标志着控制逻辑的彻底独立。
   - 核心逻辑：带外信令（Out-of-band）。
   - 原理：语音走语音的路，指令走指令的路。指令不再是简单的“脉冲”或“频率”，而是完整的计算机报文（Data Packets）。
   - 干货点：独立性：你可以一边打电话，一边收到另一封 SS7 报文（比如第二个人的呼入通知）。
   - 全球化：它让全球的交换机连成了一个巨大的计算机网络，支持了漫游、计费、800号业务。
5. 第五代：SIGTRAN / SIP —— “互联网化”这是我们第三章会讲到的内容，信令彻底拥抱了互联网。
   - 原理：把 SS7 的报文塞进 IP 包里（SIGTRAN），或者直接使用互联网协议（SIP）。
   - 现状：现代的 5G 网络中，信令已经完全变成了 API 调用。

## Multi-Exchange Network
Routes
Router
Exchange
   线路搜索器：
   - 当用户拿起电话听到拨号音时，还有一个部件开始工作。就是线路选择器Line Finders/Selector Hunters
   - 它负责把用户和交换机连接起来，然后交换机才能（才会）开始处理脉冲信号。
   - 简单来说每个用户不止和一台交换机相连，选择器会逐个扫描判断哪个交换机是空闲的，可以处理你的拨号请求。
   - 后来的多级电话网，拨出国家/市级/区级号码，其实也是一种启动选线路搜索器的步骤，你在要求国家/市区级站点，搜索可以接收处理下一段数字的交换机。
   - 不确定在直流脉冲控制步进式交换机年代就有这个部件，暂时也没有查到如果有，使用的时哪种元器件来实现此功能。


核心矛盾：为什么要用 SS7？
在早期，交换机之间通过“通话的那根线”来传指令。这导致了两个大问题：

资源浪费：如果北京的电话占线，上海的交换机得先把长途物理线路连通到北京，才能听到忙音。白白占用了昂贵的长途线路。

不安全：聪明人可以用频率发生器模拟指令，从而免费打长途。

解决方案：SS7。它把“控制信号”和“语音内容”彻底分开。这就是带外信令 (Out-of-band Signaling)。

2. SS7 网的三大“核心角色”
在信令网这个秘密社交圈里，主要有三类身份：

SSP (Service Switching Point)：这就是你熟悉的普通交换机。它负责把用户的电话接入，并向 SS7 网发问：“我这有人要打长途，怎么走？”

STP (Signaling Transfer Point)：它是信令网里的**“路由器”**。它不连电话机，只负责转发 SSP 之间的“指令包”。它就像是分拣中心。

SCP (Service Control Point)：它是网络里的**“大脑数据库”**。

干货案例：当你打 800 免费电话时，SSP 不知道该连哪。它发个信令问 SCP，SCP 查表发现 800 对应的是某公司的座机号，再告诉 SSP 怎么连。

3. 接通一通长途电话的“干货”流程
当你拨完号，还没听到回铃音时，SS7 网内发生了如下极速对话（以上海打北京为例）：

IAM (Initial Address Message)：上海 SSP 发出：“北京，我这有个上海用户（主叫）要找你那边的 8888（被叫），我预留了 15 号电路，你行吗？”

ACM (Address Complete Message)：北京 SSP 回复：“收到！被叫号码合法，我正在让他家电话响铃。你那边给主叫放回铃音吧。”

ANM (Answer Message)：北京 SSP 回复：“对方提机了！开始计费，把 15 号话路彻底接通！”

REL (Release)：通话结束，任何一方挂机，信令都会通知对方释放物理电路。

你的笔记补充建议：
Chapter 2: SS7 - The Brain of the Network

Philosophy: Separate the "Road" (Voice Path) from the "Map" (Signaling Path).

SSP, STP, SCP: The nodes that route, switch, and provide intelligence (like 800 numbers or Caller ID).

Efficiency: SS7 checks if the destination is free before occupying long-distance voice trunks.

Key Insight: SS7 is essentially a Packet-Switched Network (data packets) used to manage a Circuit-Switched Network (voice lines).


1. SS7 的“金字塔”架构
我们可以把 SS7 想象成一个专业的快递系统。每一层都只管好自己的那一环：

第一、二、三层：MTP (Message Transfer Part) —— “物流运输部”
这是 SS7 的基座，负责把信令包（也叫 MSU，信令单元）从 A 交换机搬到 B 交换机。

MTP 1 (物理层)：这就是你笔记里的 16号时隙（或者一条专门的 64kbps 链路）。

MTP 2 (链路层)：它是“押运员”。负责纠错、排序和重发。如果 16 号时隙闪断了一下，MTP 2 会确保刚才那个“接通”指令没有丢，而是重新传一遍。

MTP 3 (网络层)：它是“调度员”。它看的是 Point Code (信令点编码)。每个交换机都有个全球唯一的编号（就像邮编）。MTP 3 负责在复杂的星型网中找到路。

第四层：用户部分 (User Parts) —— “写信的内容”
这才是真正干活的内容。根据你要办的事情不同，信的内容也不同：

ISUP (ISDN User Part)：最常用。 专门负责控制语音通话。比如：“快给北京那个号码连上线（IAM报文）”。

TCAP / MAP：高级业务。 负责非通话的事情。比如：你手机开机后要向网络报告“我来北京了，请更新我的位置”（漫游），或者发一条短信。

2. 核心干货：一次“握手”的全过程
当你在上海拨打北京的电话时，SS7 网上会飞速交换以下几封信：

IAM (Initial Address Message)：

翻译：上海交换机发信：“北京，我要连 138xxxx，我这头预留了第 5 条中继线，你那头有空没？”

ACM (Address Complete Message)：

翻译：北京回信：“收到，这个号有效，我正让那哥们儿手机响铃呢（听到回铃音）。”

ANM (Answer Message)：

翻译：北京急信：“对方接听了！开始计费！”（此时语音通路正式接通）。

REL (Release)：

翻译：某一方挂断：“谈完了，把那条中继线撤了吧。”
2. 