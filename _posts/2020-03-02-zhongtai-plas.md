---
layout: post
title:  "DDD：Pace-Layered Application Strategy"
subtitle: "领域驱动设计系列"
date:   2020-03-02 07:00:00
author: "Shoukai Huang"
header-img: 'qjy1xw2zw.hn-bkt.clouddn.com/9994d024eb2eb585f1e4a974e0613bb5.jpg'
header-mask: 0.4
tags: 领域驱动
---

## Pace-layered 概念

Pace-Layered Application Strategy 是一种在整个应用程序生命周期中管理软件应用程序的方法，以支持不断发展的业务需求。

Gartner将步伐分层方法定义如下：
Gartner的 Pace-Layered Application Strategy 是一种用于分类、选择、管理和管理应用程序的方法，以支持业务变更，差异化和创新。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/cb411934d865fd425b4bb98af7406d54.jpg)

Gartner定义了三种应用程序类别或“层”，以区分应用程序类型并帮助组织为每种应用程序制定更合适的策略：

1. System of Record（SOR）：这类系统用户清楚地知道自己的需求，而且这种需求是行业或企业通用的，不需要独特性。通常是支持企业管理和交易流程的系统，如财务、HR、资产管理等。

2. System of Differentiation（SOD）：这类系统用户也清楚地知道自己的需求，战略是为了和竞争对手形成差异化，从而取得竞争优势。这类系统有可能是套装件的某个部分，更有可能是某种最佳单项产品。

3. System of Innovation（SOI）：这类系统用户不完全清楚自己的需求，要通过实验或者试错去尝试或验证，其战略是业务变革和创新。这类应用需要能被快速地构建，帮助企业捕获新的创意或机会。

这些层具有共同思想，不同思想和新思想的商业领导者的概念相对应，作为对支持系统进行分类的一种手段。System of Record（SSoR）是支持核心业务的应用程序，而 System of Differentiation 则在流程效率和有效性方面创造了竞争优势，System of Innovation 支持实现并支持业务转型的战略流程。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/00165f87b1a4fb422a9e874f1e86741f.jpg)

下表最初由Gartner在报告中设计，“如何使用 Pace-Layered 开发现代应用程序策略，”描述了不同层的特征。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/8c7317601a804ea03e1b74b6d286dd35.jpg)

## Pace-Layered 延伸

Pace-Layered Application Strategy 为“中台”产生的必然性，提供了一定程度上的理论支撑。

处于不同 Pace-Layered 的系统因为⽬的不同，关注点不同，要求不同，变化的“速率”自然也不同，匹配的也需要采⽤不同的技术架构，管理流程，治理架构甚至投资策略。

后台系统（中台概念中的后台），往往变化频率低，变化成本高，变化⻛险高，变化周期⻓。⽆法满⾜由⽤户驱动的快速变化的前台系统要求。

前台系统，为了适应业务变化及具备创新的快速响应能力，需要够⼩而美，快速迭代。

在二者不能同时满足时，在 Pace-Layered 指导思想及如下原理的推动型下，提取中台概念，或者说是建设 SOD 层。

>软件开发中遇到的所有问题，都可以通过增加⼀层抽象⽽得以解决!

![](http://qjy1xw2zw.hn-bkt.clouddn.com/7136b0c67991f20cdd1a0c7321fa9606.jpg)



## Pace-Layered 存疑

技术雷达：2015年11月

状态变更：暂缓

变更解释：Gartner的 Pace-layered Application Strategy 方法似乎无助于集中关注体系结构中的层概念。我们发现，考虑不同业务功能（可以由几个体系结构层组成）中的变化速度是一个更有用的概念。专注于层的危险是许多类型的更改跨越了多个层。例如，能够向网站添加新的股票类别不仅仅意味着拥有易于更改的内容管理系统；您还需要更新数据库，集成点，仓库系统等。认识到架构的某些部分需要比其他部分更具可操作性。但是，事实证明，专注于层次是无益的。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/a24c21853ff2da1fa46203e633cf1722.jpg)



## 参考

* [出于战略目的的应用分层推进](http://blog.sina.com.cn/s/blog_6011c34b0101lrup.html)
* [Enable a Pace-Layered Approach to Business Technology](https://d117h1jjiq768j.cloudfront.net/docs/default-source/openedge/wp_paced-layered-strategy_final.pdf)
* [Gartner's PACE Layered Application Strategy](https://cio-wiki.org/wiki/Gartner%27s_PACE_Layered_Application_Strategy)
* [白话中台战略-1开篇：中台是个什么鬼？](https://www.jianshu.com/p/86dc7ad52ad6)
* [Systems of Record and ERP](https://datachatter.wordpress.com/2015/02/18/systems-of-record-and-erp/)


