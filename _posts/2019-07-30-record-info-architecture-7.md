---
layout: post
title:  "Architecture 201907 摘录&搜索"
subtitle: "一些博客摘记"
date:   2019-06-30 10:45:13
author: "Shoukai Huang"
header-img: 'skblog.duiduiche.com/9da64338b2b8d3594483658c65b398cf.jpg'
header-mask: 0.4
tags: 读书笔记
---

本摘要为阅读 Architecture 过程中的摘录及相关搜索，书籍链接：[架构师（2019年7月）](https://www.infoq.cn/article/MV34S6hCWTT7logZt_px)

### HPCC（High Precision Congestion Control）

HPCC 含义：High Precision Congestion Control- 高精度拥塞控制。

HPCC 是在高性能的云网络环境下，对现有的拥塞控制的一种替代方案。旨在同时实现高速云网络的极致性能和超高稳定性。目前这一成果已被计算机网络方向世界顶级学术会议 ACM SIGCOMM 2019 收录，引起了国内外广泛关注。（尚在理论阶段，等待工业实现）

HPCC 的核心理念是**利用精确链路负载信息直接计算合适的发送速率**，而不是像现有的 TCP 和 RDMA 拥塞控制算法那样迭代探索合适的速率；HPCC 速率更新由数据包的 ACK 驱动，而不是像 DCQCN 那样靠定时器驱动。

来源：[改进 TCP，阿里提出高速云网络拥塞控制协议 HPCC](https://www.infoq.cn/article/q-J1qOtjcDUmYDCbWTB3)

#### The overview of HPCC framework

The key design choice of HPCC is to rely on switches to provide fine-grained load information, such as queue size and accumulated tx/rx traffic to compute precise flow rates. 

![](http://skblog.duiduiche.com/80536776015117e4f8669ee1c7875eba.jpg)

HPCC is a sender-driven CC framework. As shown in Figure, each packet a sender sends will be acknowledged by the receiver. During the propagation of the packet from the sender to the receiver, each switch along the path leverages the INT feature of its switching ASIC to insert some meta-data that reports the current load of the packet’s egress port, including timestamp (ts), queue length (qLen), transmitted bytes (txBytes), and the link bandwidth capacity (B). When the receiver gets the packet, it copies all the meta-data recorded by the switches to the ACK message it sends back to the sender. The sender decides how to adjust its flow rate each time it receives an ACK with network load information.

来源：[HPCC: High Precision Congestion Control](https://schwartzpr.de/website/uploads/Alibaba-HPCC-High-Precision-Congestion-Control.pdf)


### TCP 拥塞控制算法

拥塞控制是一种用来调整传输控制协议（TCP）连接单次发送的分组数量（单次发送量，在英文文献和程序代码中常叫做cwnd）的算法。它通过增减单次发送量逐步调整，使之逼近当前网络的承载量。如果单次发送量为1，此协议就退化为停等协议。单次发送量是以字节来做单位的；但是如果假设TCP每次传输都是按照最大报文段来发送数据的，那么也可以把数据包个数当作单次发送量的单位，所以有时我们说单次发送量增加1也就是增加相当于1个最大报文段的字节数。

**算法**

拥塞控制假设分组的丢失都是由网络繁忙造成的。拥塞控制有三种动作，分别对应主机感受到的情况：

1. 收到一条新确认。这很好，表明当前的单次发送量小于网络的承载量。
2. 收到三条对同一分组的确认，即三条重复的确认。单次发送量往往大于3，例如发送序号为0、10、20、30、40的5条长度为10字节的分组，其中序号20的丢了，则返回的确认是10、20、20、20。3个20就是重复的确认。
3. 对某一条分组的确认迟迟未到，即超时。例如发送序号为0、10、20、30、40的5条长度为10字节的分组，其中序号30的丢了，则返回的确认是10、20、30、30。这才只有两条重复确认。然而刚刚说过，单次发送量往往大于3，所以超时更可能是因为不止一条分组或确认丢失而引起的，这说明网络比上一情况中的更加繁忙。

当主机收到一条新确认，此时可以增加单次发送量。若当前单次发送量小于倍增阈限（在英文文献和程序代码中常叫做ssthresh），则单次发送量加倍（乘以2），即指数增长；否则单次发送量加1，即线性增长。

当主机收到三条重复的确认——单次发送量减半，倍增阈限等于单次发送量。（进入线性增长期）

当主机探测到超时——倍增阈限=单次发送量÷2，单次发送量=1。

来源：[拥塞控制](https://zh.wikipedia.org/wiki/%E6%8B%A5%E5%A1%9E%E6%8E%A7%E5%88%B6)

### SIGCOMM

创立于1947年的ACM是全世界计算机领域影响力最大的专业学术组织。ACM所评选的图灵奖（A.M. Turing Award）被公认为世界计算机领域的诺贝尔奖。ACM目前在全世界130多个国家和地区拥有超过10万名的会员。SIGCOMM会议是ACM在通信网络领域的旗舰型会议，也是国际通信网络领域的顶尖会议。

来源：[全球网络通信顶级会议ACM SIGCOMM 在京举行](http://www.edu.cn/info/focus/xs_hui_yi/201908/t20190820_1679055.shtml)

SIGCOMM 是 ACM 组织在网络领域的旗舰型会议，也是目前国际网络领域的顶尖会议。几十年以来，多项经典研究成果都出自 SIGCOMM 大会，比如《Development of the Domain Name System 》(SIGCOMM 1988)，阐述了互联网域名管理系统（DNS），这套系统已经被使用几十年，贯穿了互联网的发展史；《Congestion Control in IP/TCP Internetworks 》(SIGCOMM 1987) 和《Congestion Avoidance and Control 》(SIGCOMM 1988)，奠定了互联网 TCP 拥塞控制的基础，其算法设计思想一致沿用至今； 《Ethan: Taking Control of the Enterprise 》(SIGCOMM 2007)，软件定义网络 (SDN) 思想的开山之作，SDN 使得大规模网络虚拟化成为可能，让“云网络”的概念落地。

来源：[改进 TCP，阿里提出高速云网络拥塞控制协议 HPCC](https://www.infoq.cn/article/q-J1qOtjcDUmYDCbWTB3)

### 职业发展体系

阿里巴巴集团采用双序列职业发展体系，技术线就是常说的 P 序列，对应到管理线的 M 序列，P6 相当于 M1，P7 相当于 M2，以此类推。

![](http://skblog.duiduiche.com/d1c5639c01a00d0c195124403742fcb2.jpg)

目前阿里需求量最大的职级范围分布在 P6-P8，这也是阿里集团占比最大的级别。P6 级别的程序员 title 是高级工程师，P7 便已经是专家级别，P8 则是高级专家。一般而言，江湖上行走小有名气的阿里程序员至少也是 P8 级别。P10 级别的存在就是传说中的大神级别，这个级别的程序员无一不是业界鼎鼎有名的存在，比如褚霸、毕玄等等。

InfoQ 搜集了阿里巴巴职级体系下的薪资水准和股数，具体参考下表：

![](http://skblog.duiduiche.com/064547adec3147f00444fdb7de26b00f.jpg)

在腾讯，技术线在此之前属于 T 序列，在腾讯的职级体系里，T3 级别已经是很多人的上限，行走江湖有名有号的 T4 级别更是当得起各技术分享大会的技术爱好者们一声老师的称呼。而 T5 级别在整个腾讯也是凤毛麟角，代表人物有玄武实验室的于旸、优图实验室的贾佳亚等。

InfoQ 搜集了腾讯新职级体系下的薪资水准和股票价值，具体参考下表：

![](http://skblog.duiduiche.com/3d550df05c94a11b6ff4195ca297948f.jpg)

百度是整个 BAT 中现金给得最多的。

和腾讯相同，百度技术线也是 T 序列，T5、T6 是技术线占比最大的级别。一般而言，在百度 T5 是高级工程师、T6 是资深工程师，但实际上百度的 title 并没有职级重要。从 T7 级别开始，就开始要做带团队、做管理的事情，升到 T7 以上后基本就不做写代码的事情了。T10-T12 的人数非常少，具有代表性的人物有前百度首席科学家吴恩达、百度最年轻 T10 楼天城等。

InfoQ 搜集了百度职级体系下的薪资水准和股票价值，具体参考下表：

![](http://skblog.duiduiche.com/1922e655394d7589e75d66ed161d1aeb.jpg)

华为技术线的职级体系为数字序列，跟腾讯的新序列相近。华为有句俗语很好地描述了收入情况：三年一小坎，五年一大坎。意思是入职华为三年内大部分靠工资，三年后奖金逐步可观，五年后分红逐步可观。事实上，根据 InfoQ 调查了解到的情况也确实如此，在华为供职年限越久，奖金越多，分红规模越大。2015 年，现任华为高级副总裁陈黎芳在北大宣讲时提到：奋斗越久越划算，工资变成零花钱。

InfoQ 搜集了华为职级体系下的薪资水准和股票价值，具体参考下表：

![](http://skblog.duiduiche.com/ca4cae01c01d4093bb087e193f9efe62.jpg)

互联网公司的职级，以前我们只能看个热闹，现在我们终于也能看个门道了。其实在技术发展的路线上，慢慢也出现了一个名叫“职业阶梯”的名词。制定职业阶梯的目的是让那些有才华的技术人在职业上有更多的成长和晋升可能性，同时又不需要让他们走管理路线。职业阶梯目前在硅谷已经较为流行，随着互联网技术在中国的持续发展和繁荣，西学东渐，未来的中国技术人肯定也能一直写代码写到 5、60 岁以后。


来源：[阿里P10、腾讯T4、华为18，互联网公司职级、薪资、股权大揭秘](https://mp.weixin.qq.com/s?__biz=MzIzNjUxMzk2NQ==&mid=2247491347&idx=2&sn=432d65ccf4a68043f839c4d0d9eac103)



