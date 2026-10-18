# Chapter 2: Control Logic & Signaling
如何让庞而大复杂的电话网络，知道“张三现在要连李四”，并准确无误地给他们俩建立一条通路？

## Control Logic
如果把拨打电话看作发出**指令**——“我要和李四通话”，那么谁来执行这条指令？\
分几步？每步由谁来完成？怎么做？如何能保证又快又不出错？

### Common Control Subsystem 公用控制子系统
就像所有的标准工业化生产逻辑，我们需要梳理出一套标准操作方案——**Common Control System 公用控制系统**。完成**识别打电话和接电话的人**并**找到负责接通电话的所有交换机**这两部分重要工作。
- 监控模块：
  - 每个模块之间发生的事件。比如在线路上监控的是有呼叫、挂断电话。
  - 不是监控其他模块的功能运转，如单个交换机是如何完成连接动作的。
  - 监控点包括：线路上，连接点，内部信号收发端。
- 呼叫处理模块：
  - Digit Receiver & Storage Register 数字（信号）接收器，寄存器 register。用于接收拨出的电话
  - Initial Translator 发起连接解释器，用来做内部连线方案的翻译，询问要走哪条线路，怎么建立连接，rate(可能指电话费怎么算）等
  - Final Translator 建立连接解释器，用来把接收到的号码信息拆解翻译成接下来要连接的各段线路的机器码
  - Register Sender 寄存信息发送器，会把打电话的人和接电话的人的唯一编号，用合适的信号方式送出去
  
- 计费模块
  - 有些电话拨打永远是免费的，如公共服务电话
  - 而不同用户打电话的费率也可能是不同的

- 运营和维护模块
  维护和运行分以下两中模式（主要区别就是路由表交换机里，还是在公用控制单元里）：
  - Map-in-Memory：交换机根据自己的control unit控制模块里的路径地址表（路由表？）找到最优线路的二进制编码。
  - Map-in-Network：指最优线路的二进制码存在公用控制单元common control unit一层。这在纵横中继网Crossbar exchanges中非常常见。

- 管理和维护模块
  包括新用户建立，扩容，更改现有用户的消费级别，更新线路选择方案。维护人员要关注每个部件是否正常工作，并定期做校验。

### Stored Program Control - SPC 程控系统
SPC is the concept of electronics that ringed in a change in telecommunication, where a program or a set of instructions to the computer is stored in its memory and the instructions are executed automatically one by one by the processor.\
线路中传送的不再是声波模拟信号（音频）而是数字离散信号（震荡的电子）。程控系统是由一套预先存在交换机里的，会自动从头至尾执行的程序，来实现接电话，监控，计费，系统维护等操作。
- 一个或多个SPC负责接线，一个单独的SPC负责维护
- Centralized SPC
  - 最早是一个主SPC完成所有工作，后来由双SPC一起执行任务，互相备份
  - Standby Mode 待机模式：如果一个报错了，叫另一个上
  - Synchronous Duplex Mode 同步双工模式：一起算，如果结果不一样启动纠错程序
  - Load Sharing Mode 负载共享模式：接线任务被分给两个做，如果一个挂了，就把它那份转交给另一个做，但一般是两个均分任务。
  
- Distributed SPC
  - Regional Processors 每个区块只负责处理一种事情
  - 可以是让不同区块只负责处理固定机器发来的接线任务（纵向切分）
  - 也可以把任务本身切成几个步骤，每个步骤交给一个区块去处理（横向切分）


## Signaling 
让机器干活，确实不像给人交代工作那样随意。每种机器都指接受适合它的那种信号。随着整个控制系统越来越自动化、功能越来越多，发令的方式，指令的内容都要跟着升级。

### History of Signaling
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
   
   指令传输和通话用一条物理线路实现：
   - 拨号的时候，你听到的“嗒嗒嗒”声就是指令。
   - 速度极慢，且只能传简单的数字，没法传“呼叫转移”或“来电显示”等复杂指令。
   - 在旋转拨号盘上拨打一个7位数字的电话号码需要12秒。这套电磁机械控制的交换系统，如果每秒收到超过10-12个的脉冲信号，就会出错（棘轮转不过来，连到错误的线路）
   
2. 第二代：双音多频 DTMF 和通用控制系统 Common Control Subsystem\
   随着用户数量的增加，电话网络变得庞大，进而发展出多级交换网络。让每个用户了解应该拨通哪一个中继站exchange才能完成呼叫工作是不现实的。\
   通用控制子系统被引入：
   - 由机器记住电话网络结构，
   - 机器之间直接沟通
   - 找到理想连接线路
   - 给接打电话的人建立通路
   - 同时还能承担计费等其他工作\
   
   这个时候的信令控制也随之升级成了可以支持这种通用控制系统的方式。\
   收到信号和处理连接两个事情被彻底分开，也就不再需要拨号码的人控制发出脉冲的速率。\
   拨盘电话Pulse dialing可以升级成按键电话Touch-tone dialing。\
   交换机基本上是纵横交换机Crossbar Switching。
   
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

   控制流程（简述）：
   - 拿起电话：电话机发出第一个信号，线路选择器开始工作选择一个空的寄存器Register，返回拨号音
   - 拨电话：寄存器记下收到的合格双频信号，然后送到解释器，机器码被解释出来，交换机开始寻找最优线路解
   - 听到响铃：交换机找到了最优线路，并且指挥Crossbar Switching把线路连通，送出信号让被呼叫的电话开始响铃，也返回给拨电话一个播放响铃音信号
   - 对方接起电话，监控模块也检测到这个事件，计费工作开始
   - 电话挂断，监控模块收到消息，断开了这个临时通路，结束计费。

   Crossbar Switching 纵横交换机（网络）工作原理
   - 结构，横线 x 纵线。每根横线和纵线都连着一个终端用户或另一个节点，Crosspoint交叉点上是连接开关
   - 简单来说把交叉点的开关闭合，就把线路接通了。但要保证一根横线一次只能和一根纵线连接。
   - 整个纵横交换机（网络）的设计难点在于，在保证提供同时接打电话线路的数量的前提下，如何减少交叉点开关的数量，然后让接通开关这个工作完成的更快，更经济。
   
   端到端的带内信令（End-to-End & In-band signaling）：
   - 不同于脉冲拨号时代，信号只需要从终端送至中继站exchanges，音频拨号时代的信号需要在整个交换网络中来回传递（End-to-End 端到端）。
   - 信号频率需要在声音的频率范围内，这样才能通过为传输声音设计的线路中被传递。
   - 安全性差，可被模仿。
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