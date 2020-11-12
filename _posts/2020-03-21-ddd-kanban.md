---
layout: post
title:  "DDD：看板"
subtitle: "领域驱动设计系列"
date:   2020-03-21 08:00:00
author: "Shoukai Huang"
header-img: 'qjy1xw2zw.hn-bkt.clouddn.com/5f8176627139f995349fcb0da380b308.jpg'
header-mask: 0.4
tags: 领域驱动
---

## 定义

wikipedia 定义

> Kanban (看板) (signboard or billboard in Japanese) is a scheduling system for lean manufacturing and just-in-time manufacturing

atlassian 定义

>A kanban board is an agile project management tool designed to help visualize work, limit work-in-progress, and maximize efficiency (or flow). Kanban boards use cards, columns, and continuous improvement to help technology and service teams commit to the right amount of work, and get it done!

David J. Anderson 定义

>看板定义了我增量、渐进地改变技术开发和组织运营的方法。它的核心机制是限制在制品数量的拉动系统，通过它暴露系统运作（或流程）的问题，并激发协作以改进系统。

Kanban看板是一种可视化生产管理系统，利用看板卡来增强信号量、标记生产过程，促进系统渐进式变化，提高团队协作的效率。


## 基础

核心理论：流动性、可视化

### BASIC of Kanban：流动性

Cycle Time = Work in Progress / Throughput

Kanan 看板系统的基础理论是利特尔法则（ Little’s Law），由MIT （Sloan School of Management）的教授John Little于1961年提出：在一个稳定的系统 L中，长期的平均顾客人数，等于长期的有效抵达率，系统中的平均存货等于存货单位离开系统的比率（亦即平均需求率）与存货单位在系统中平均时间的乘积。

>the relationship between the average number of customers in a store, their arrival rate, and the average time in the store.

根据利特尔法则，跟踪工作及其进展的最重要的目标是：限制在制品（Work in process，WIP），例如尚未完成制造过程的商品，或是停留在库存仓库或是产线上，等待着进一步处理的商品。

> 把工作拆散以适应任意的时间节点，对完成速率、避免缺陷和团队士气具有深远的负面影响。不必要的截止日期压力或过于乐观的特性范围对团队和产品都会产生负面作用。这时工作重心就变成了将工作挤压到截止日期前而不是高质量地完成工作。高质量地完成工作只有在工作以可持续的节奏流动时才有可能。发现并维持着一节奏只有在制品小于团队产能的情况下才有可能。在截止日期之前塞入太多工作几乎不可避免地会打破在制品限制。。— Jim Benson 《Personal Kanban》作者

### BASIC of Kanban：可视化

> 像看板这样的可视化系统因我们对视觉信息的偏好而成为了强有力的工具。真实地看到工作和流程有助于理解。看板墙可以作为一个简单的信息节点，使任何人都能走过来并了解项目的当前状态。 《Personal Kanban》作者 Jim Benson

Kanban看板可视化的一些技巧：

* Kanban看板墙需放置于工作区醒目位置
* 不同事件类别使用不同颜色，紧急事件（URGENT）使用醒目颜色（红色）
* 信息卡片常规要素：编号，标题，负责人，截止日期
* 信息卡片叠加要素：重要度，约束条件，延期原因等特殊描述
* 定期回顾（周），及时复盘总结（月）
* 限制进行中（In-Process）事件数量，限制已经完成（Done）事件数量（折叠或者更换新的Kanban看板墙）


## 元素

David Anderson 分解看板为：

* Visual signals 
* columns
* work-in-progress limits：WIP limits are the maximum number of cards that can be in one column at any given time （WIP限制是在任何给定时间可以在一列中的最大卡数）
* a commitment point：Kanban teams often have a backlog for their board. 
* a delivery point：The delivery point is the end of a kanban team’s workflow

![](http://qjy1xw2zw.hn-bkt.clouddn.com/f4b5f8ec8e6063c0e338ed05741cd984.jpg)

## 核心实践

kanbanize 公司的 The 4 Kanban Core Practices 

* Principle 1: Start With What You Do Now
* Principle 2: Agree to Pursue Incremental, Evolutionary Change
* Principle 3: Respect the Current Process, Roles & Responsibilities
* Principle 4: Encourage Acts of Leadership at All Levels

kanbanize 公司的 The 6 Practices of Kanban

1. Visualize the Workflow: 
2. Limit Work in Progress: One of Kanban's primary functions is to ensure a manageable number of active items in progress at any one time. 
3. Manage Flow: Managing the flow is about managing the work but not the people. By flow, we mean the movement of work items through the production process.
4. Make Process Policies Explicit: You can’t improve something you don’t understand. This is why your process should be clearly defined, published, and socialized.  
5. Feedback Loops: For teams and companies that want to be more agile, implementing feedback loops is a mandatory step. They ensure that organizations are adequately responding to potential changes and enable knowledge transfer between stakeholders.
6. Improve Collaboratively (using models & the scientific method): Teams with a shared understanding of their goals, workflow, process, and risks are more likely to build a shared comprehension of a problem and to work together towards improvement.


何勉整理的看板核心实践

* 核心实践 1：可视化工作（价值）流：看板开发方法把可视化工作流作为基础实践，先让价值和价值流动具体可见，然后才是管理和优化。
* 核心实践 2：显式化流程规则：指明确定义和沟通团队所遵循的流程规则
* 核心实践 3：限制在制品数量：限制在制品数量是看板开发方法的核心机制，与产品制造类似，通过拉动系统可以：加速价值流动、暴露问题
* 核心实践 4：度量和管理流动。度量不是目的，而是管理和优化价值流的手段。对于特定的组织，度量什么应该由目标和现状决定。
* 核心实践 5：协同改进（Improve Collaboratively）：解决瓶颈问题的方案可能在瓶颈处，如临时加班、分配更多资源、或相邻环节的支持等。但很多时候解决瓶颈问题的方案在别处，例如提高瓶颈之前环节的输出质量，调整职责分配，甚至是重新设计工作流。

kanban-in-action 一书总结的六个实践

* 1 Visualize—Described earlier.
* 2 Limit work in process—Described earlier.
* 3 Manage flow—Described earlier.
* 4 Make process policies explicit—With explicit policies, you can start to have discussions around your process that are based on objective data instead of on what you think, feel, and have anecdotal evidence for.
* 5 Implement feedback loops—This practice puts a focus on getting feedback fromyour process: for example, in what is called an operations review, which is a kind of retrospective for the process itself.
* 6 Improve collaboratively, evolve experimentally (using models and the scientific method)—This practice encourages you to use models such as the Theory ofConstraints or Lean to push your team toward further improvements.


## 参考

* [What is a kanban board?](https://www.atlassian.com/agile/kanban/boards)
* [看板实践——前言(笔记)](https://www.cnblogs.com/aixiaoxiaoyu/p/10280598.html)
* [DevOps 漫谈:看板Kanban管理实践](https://riboseyim.github.io/2017/08/06/TeamWork-Kanban/)
* [Kanban Explained for Beginners - The Complete Guide](https://kanbanize.com/kanban-resources/getting-started/what-is-kanban)
* [解析精益产品开发（一）—— 看板开发方法](https://www.infoq.cn/article/kanban-development-method/)
* [kanban-in-action](http://3.droppdf.com/files/p99PT/kanban-in-action.pdf)

