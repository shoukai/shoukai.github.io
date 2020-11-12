---
layout: post
title:  "DDD：Operating Model（运营模式）"
subtitle: "领域驱动设计系列"
date:   2020-03-02 08:00:00
author: "Shoukai Huang"
header-img: 'qjy1xw2zw.hn-bkt.clouddn.com/aefd421cdbaef80e8bf47fe8dc87d0e2.jpg'
header-mask: 0.4
tags: 领域驱动
---

## Operating Model 相关概念

### 定义

运营模式是一个组织如何提供价值给客户或受益人以及如何组织实际运行本身既是抽象的视觉表现（模型）

有多种方法来定义构成操作模型的元素。

人员，过程和技术是一种常用的定义，过程，组织和技术是另一种定义，过程、组织、位置、信息、供应商、管理系统是另一种。

>People, process and technology is one commonly used definition,process, organization and technology is another, and Process, Organization, Location, Information, Suppliers, Management system yet another.

### 作用

组织是传递价值的复杂系统。运营模型将这个系统分为几个部分，显示了它是如何工作的。它可以帮助不同的参与者了解整体。它可以帮助领导者识别导致绩效下降的问题。它可以帮助进行更改的人员检查他们是否已考虑所有要素，并且整个要素仍将起作用。它可以帮助那些转换操作的人员协调所有需要发生的不同变化。

### 限定

术语“运营模型”可能首先在公司级战略中使用，以描述组织到业务部门的组织方式，集中或分散活动的范围以及跨业务部门需要多少集成。今天，当指单个业务部门或单个职能部门的运作方式时，例如“勘探部门的运营模型”或“HR 职能部门的运营模型”中，该术语最为常用。它也可以在更微观的层面上用于描述功能部门的工作方式或工厂的布局，用于思考不同公司策略对IT的影响。

## Operating Model in IT

在MIT中心信息系统研究（CISR），在一个研究小组麻省理工学院 斯隆管理学院，建议的运营模式是非常有用的指导IT投资决策。IT投资必须支持运营模式。

Ross，Weill和Robertson发现，具有运营模式的组织报告其运营效率提高了31％，客户满意度提高了33％，在新产品开发方面的优势为34％。在《作为策略的企业体系结构》一书中，他们概述了四个操作模型：

![](http://qjy1xw2zw.hn-bkt.clouddn.com/3b7d63182e6755ebeb238417ed29039d.jpg)

### 协调（Coordination Operating Model）

低流程标准化但高流程集成。协调运营模型的特征是共享客户，产品或供应商数据，但在运营上具有独特性，可能会影响彼此的交易。这些自主的业务部门对业务流程设计具有高度的控制权，以适应其特定的操作。下图直观地表示了协调操作模型。

### 统一（Unification Operating Model）

高标准化和高集成度。统一运营模型基于一组全球集成的业务流程，其中客户和供应商在地理上分布。业务部门具有类似的操作，其中集中设计了流程和数据，以便可以共享它们。这些流程的集​​中管理通常利用矩阵方法来跟踪业务部门组成。尽管业务部门具有不同的运营，但是高级业务流程所有者致力于使整个业务部门的业务流程标准化。本质上，统一是基于一组规范的过程和数据，这些过程和数据可以动态配置为在每个业务部门的操作中执行。

### 多元化（Diversification Operating Model）

要求低标准化和低集成度的业务。多元化的基础是业务部门几乎没有共享的客户或供应商（如果有）。这些业务部门在运营上也是唯一的，并且具有独立的交易。多元化运营模型中的业务流程标准化和集成最少。大多数IT决策和业务流程设计是在每个业务部门制定的。但是，这些业务部门确实利用了一组可以集成到其特定环境中的通用共享服务。

### 复制（Replication Operating Model）

标准化程度高，但集成度低。复制操作模型也几乎没有共享的客户或供应商。复制操作模型中的自治业务部门利用联合方法来进行业务流程集成和标准化。业务流程设计和IT服务都是集中管理的。信息体系结构使用规范的数据定义进行了标准化，但是实际数据是本地拥有的，并且对企业具有一定的聚集性。从运营的角度来看，业务部门的执行非常相似。

### 总结

运营模型告知业务流程集成和标准化的适当级别，以将组织的承诺交付给利益相关者。与多元化和复制模型相比，协调和统一模型从整个企业的客户和数据的整合视图中受益更多。

## Operating Model 延伸

运营模型的两个象限：

横轴代表标准化（Standardization），标准化越高，你可以简单理解成企业就是通过业务模式（业务功能 + 业务流程）的复用实现业务线的扩展。比如像电商网站的各个垂直网站，或是全球化，都是通过将电商的业务模式复用，通过复用到不同的垂直领域，或是不同的地区来实现不同业务线的扩展。

纵轴代表集成（Integration），也可以理解成数据集成，这种运营模式的企业就是通过对数据的复用，来实现业务线的扩展的。比如最常见的像腾讯，通过对微信用户数据的集成和复用（导流），来帮助新的业务线（例如游戏）快速扩展。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/96cd3422ce7ca0f9d231d3635bafec0e.jpg)

那为什么要提到这个模型呢，因为这里的“商业运营模型”就很像我们中台里的能力复用方式，即你复用的到底是业务模式；还是复用的是业务数据；还是两者都不是，只是复用了更底层的技术部分。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/f3a9c198009aab66e9875f321a68fab4.jpg)

## 参考

* [极客时间：说透中台](https://time.geekbang.org/)
* [Operating model](https://en.wikipedia.org/wiki/Operating_model)
* [Selecting an Enterprise Operating Model Based on the Business Model Design](https://labs.sogeti.com/selecting-an-enterprise-operating-model-based-on-the-business-model-design/)


