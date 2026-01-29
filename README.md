这一篇我自己设计的系统研究通信工程的学习笔记。

现代互联网的很多核心概念（如交换、多路复用、信令）其实都是从百年电话史中演化而来的。
我希望将通信工程（底层的信号与电话网）和计算机网络（高层的互联网与上网）的工作原理都了解清楚。
所以我和AI聊了聊，制定了一份学习大纲。

# Telecommunication Systems
From Switching Networks to IP Routing

## [第一章：The Foundation of Switching (交换基础)](/switching.md)
- 为什么需要交换机？（解决 $N$ 对 $N$ 的连线灾难）。
- 交换机组成的网络：从街道端局（End Office）到国家中心局。
- 如何帮打电话的人连上接电话的人

## [第二章：Control Logic & Signaling (控制与信令)](/signaling.md)
- 什么是控制系统：如何处理占线、如何计费、如何跨国追踪一个呼叫。 
- 信号是怎样指挥的：交换机如何根据拨号频率（脉冲/双音多频）识别号码并选择出口。
- SS7 (7号信令)：这是通信工程最迷人的部分。它是网络背后的专用控制网。

## 第三章：Evolution to IP (向 IP 的演化)
- 分组交换逻辑：为什么要把连续的通话切成“包”？
- 寻址转换：电话号码（E.164标准）与 IP 地址的逻辑异同。
- VoIP 实操：现在的手机语音是如何跑在互联网协议之上的。

## Signal Processing 信号处理
- 解决的是： “怎么把信息变成能跑的东西？”
- 干货点： 模拟信号 vs 数字信号、电缆/光纤的频率、衰减、调制（Modulation）。
- 解决有线电话里声音如何变成电流。

这部分暂时不多研究。


# 随着学习，我发现的宝藏知识库们
## 视频
- 📽️️[AT&T的Switchboards发展史视频](https://www.youtube.com/watch?v=xJ1fKFqt7qU)
- 📽️️[西雅图的Connections Museum介绍](https://youtu.be/D_m4K5Q5yRU?si=GE8wJw6AUbprgkzN)
- ▶️[博物馆自己的频道](https://www.youtube.com/c/ConnectionsMuseum)
- 📽️[Step-By-Step Telephone Swith](https://youtu.be/VN_K2PgMYq8)
- 📽️[Video:Panel Telephone Switch](https://www.youtube.com/watch?v=Gsx2ZsYggGw)
- 📽️[A Crossbar Telephone Switch Explained](https://hackaday.com/2024/02/04/a-crossbar-telephone-switch-explained/)
- ▶️[一个技术宅发的老电话系列视频(英文)](https://youtube.com/playlist?list=PLy4HJ7g3Ybft4ANU15k1Z7dSj6cdOiA12&si=qK4r3tOjhNyUMcil)

## 系统教学网站
- 👉[TSSN电信交换系统教程网站](https://www.w3ccoo.com/telecommunication_switching_systems_and_networks/index.html)
- 👆[TSSN原始英文教程](https://www.tutorialspoint.com/telecommunication_switching_systems_and_networks/index.htm "能看英文看英文，比中文好很多")
👈[【离线网页版】](/TSSN_Quick_Guide.mhtml)

## 通俗易懂的文章
- 📃[电话交换百年史——交换机](links/电话交换百年史_交换机.mhtml)
- 📃[现代交换原理CH2 TST网络](https://blog.csdn.net/grin99/article/details/104948967)

# AI推荐的教程
## 核心入门：通信工程的“灵魂”
理解“交换网络”
### 书籍推荐：《通信新读：从原理到应用》 (作者：陈福彬) ❌太深太数学
- 理由：这是目前国内市面上唯一一本完全不讲复杂公式，却能把整个通信系统（从有线电话机到基站）讲得清清楚楚的“神书”。 
- 干货点：它会详细解释什么是交换（Switching），号码是怎么管理的，以及电话网如何一步步变成了互联网。非常契合你“不花太多时间在信号处理”的需求。
### 视频推荐：B站《现代交换原理》（北京邮电大学 卞佳丽）❌无字幕太冗长
- 理由：北邮是通信界的黄埔军校，卞老师的课非常经典。
- 观看建议：跳过涉及具体芯片时序和复杂计算的章节。重点看：电路交换原理、层级交换网、信令系统（SS7）。看完这些，你就懂了成千上万号码是怎么管理的。

## 核心进阶：互联网的“骨架”
理解了交换（连接的逻辑）后，接下来就要看这些逻辑在互联网里是如何变身的。
### 书籍推荐：《计算机网络：自顶向下方法》 (Computer Networking: A Top-Down Approach)
- 理由：这本书是全球标杆。之所以推荐它，是因为它从应用层（你熟悉的网页、视频）开始讲，最后才讲到底层，这种逻辑非常符合人类认知。
- 干货点：你可以重点看 Network Layer（网络层） 这一章，对比 IP 路由和电话交换的不同。
### 视频推荐：B站《计算机网络》（中科大 郑烇）
- 理由：郑老师的课非常有深度，他不仅讲“是什么”，还讲“为什么这样设计”。
- 观看建议：重点看路由算法、子网划分、以及 DNS（域名系统）。你会发现，DNS 其实就是互联网时代的“超级电话簿”。

## 实战工具：让你“看”到交换逻辑
光看书太抽象，你需要两个免费的工具来“实操”一下：
### Cisco Packet Tracer (思科网络模拟器)
- 怎么玩：你在里面放两台电话（模拟电话网）或者两台电脑（模拟互联网），亲手连上电线，配置一下网关。当你看到信号包在节点间跳动时，你会瞬间明白什么叫“寻址”。
### Wireshark (抓包神器)
- 怎么玩：在你自己电脑上运行它，然后打开一个网页。你会看到成百上千条信息。这就是“分组交换”的真面目——你的所有动作都被切成了带标签的小碎块。