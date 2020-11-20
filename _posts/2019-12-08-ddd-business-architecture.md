---
layout: post
title:  "DDD：Business Architecture（业务架构）"
subtitle: "领域驱动设计系列"
date:   2019-12-07 08:00:00
author: "Shoukai Huang"
header-img: 'cdn.apframework.com/e40dc5be8ba4d924cec7641c286e5bf3.jpg'
header-mask: 0.4
tags: 领域驱动
---

## 业务架构演进

Zachman framework。虽然没有明确提出业务架构这个概念，但是已经包含了业务架构关注的一些主要内容：如流程模型、数据、角色组织等，既然没有提出业务架构概念，自然也就没有包含构建方法。Zachman 模型应该算是业务架构的开端，同时，它也表明了这一工具或者技术的最佳使 用场景——面向复杂系统构建企业架构。

TOGAF 进一步认为企业架构分 为两大部分:业务架构和 IT 架构，大部分企业架构方法都是从 IT 架构发 展而来的。业务架构是把企业的业务战略转化为日常运作的渠道，业务战 略决定业务架构，它包括业务的运营模式、流程体系、组织结构、地域分 布等内容。TOGAF 强调基于业务导向和驱动的架构来理解、分析、设计、构建、集成、扩展、运行和管理信息系统，复杂系统集成的关键，是基于架构(或体系)的集成，而不是基于部件(或组件)的集成。

TOGAF 之后，又先后诞生了FEA(联邦企业架构)和 DODAF(美国国防部体系架构框架)。前者的体系由五个参考模型组成:绩效参考模型(PRM)、业务参考模型(BRM)、服务构件参考模型(FRM)、数据参考模型(DRM)、技术参考模型(TRM)，该方法应用于美国电子政务领域，着眼于跨部门、跨机构提升业务效率，解决重复建设、信息孤岛等问题，很具有“企业级”理念，虽然没有明确的业务架构定义，但是很好地应用了业务架构的思维。

## 业务架构定义

业务架构就是对业务的结构化表达。至于什么样才算“结构化表达”。

《企业级业务架构设计》给业务架构提供了一个简单的定义：
>以实现企业战略为目标，构建企业整体业务能力规划并将其传导给技术实现端的结构化企业能力分析和方法

BIZBOK 给出了一个更加具象化的定义，即：
>业务架构呈现全面的、多维度的业务视角，包括：业务能力、端到端的价值交付、信息和组织结构，以及这些业务视角之间的关系，以及它们与战略、产品、政策、项目和Stakeholder之间的关系

BIZBOK是业务体系结构知识体系（Business Architecture Body of Knowledge）的首字母缩写。它是由Business Architecture Guild设计的流行的业务架构最佳实践框架。

![](http://cdn.apframework.com/3576f82a5529de41ddbf44e9fb4fb585.jpg)


BIZBOK存在三个重要元素：**抽象，领域和蓝图**。

如上所述，Abstractions（抽象）是**业务的形式表示**。这些视觉表示包含域。领域是代表**操作中的概念的单个单元**（如策略和指标）。

>Abstractions, as mentioned above, are the formal representation of the business. These visual representations comprise of domains. The domains are the individual units that represent concepts in operations, like strategy and metrics.

抽象显示了具有相互关联的目标和结果的多个领域之间的联系。领域要求领导者考虑将自己的业务分解为多个单元。一旦定义了单位，就可以将其转换为构成业务体系结构抽象的域。

>An abstraction shows the connections between several domains with interlocking goals and outcomes. Domains challenge leaders to think of their business broken down into units. Once the units are defined, they can be transformed into domains that make up an abstraction of the business architecture.

下图是BIZBOK的业务架构模型，其中内环四个是核心元素，即：

* Information（业务概念）
* Capability（业务能力）
* Value Stream（价值流）
* Organization（组织）

外环是六个扩展元素，即

* Strategies（战略）
* Initiatives（项目）
* Metrics（指标）
* Products（产品）
* Policies（政策）
* Stakeholder（干系人）

这些见解的共同点是，它们都相互依靠才能取得成功。**蓝图可帮助利益相关者熟悉抽象的最重要部分，回答特定问题或解决方案**。实际上，它们通过添加上下文使抽象和领域有意义。

>What these insights have in common is that they all rely on one another to be successful. Blueprints help stakeholders hone in on the most important parts of the abstraction, answering a specific question or resolving a scenario. In effect, they make sense of abstractions and domains by adding context.

## 业务架构细化

利益相关者还将面临从每个领域中得出四个insights（见解）的挑战：业务概念、业务能力、价值流、组织。

![](http://cdn.apframework.com/d5b559e205a95e6edad46cbc947ebd44.jpg)

### Information（业务概念）

**业务概念是将现实世界中的人、事、物进行抽象，并映射到数字世界的一种载体，核心是“抽象”。**

这里的抽象不是过程抽象，而是本质抽象。例如：产品需求要评审，合同条款也要评审，但“评审”不是业务概念（而是一个操作），“产品”和“合同”才是业务概念。
业务概念的作用在于确保语义的一致性。例如，不同业务领域都存在“需求”这个术语。销售人员所说的需求与开发人员所说的需求可能非常不一样，前者一般是指客户面临的问题或挑战（wants and needs），对象是客户；而后者一般是指产品需求（requirement），对象是产品。在一个需求管理相关的软件项目中，如果设计人员不能准确区分这两个概念，可能导致大量无效需求进入开发环节。而这恰恰是业务概念要解决的问题。

![](http://cdn.apframework.com/e617005e61447eb50e7619833ca1d72d.jpg)

### Capability（业务能力）

**把一个业务概念和与之相关的操作（或行为）封装起来，就成了业务能力。**

这里的操作不是指增、删、改、查之类的数据库操作，而是真实业务中对业务概念的操作。例如：员工（Human Resource）是一个业务概念，与之相关的操作包括员工权限管理、员工绩效管理、员工薪酬管理、员工风险管理、员工偏好管理，等等。

业务能力的作用在于模块化。它描述业务“做什么”，同时屏蔽了业务要“怎么做、什么时候做、谁来做”等细节，从而大大提高了共性、稳定性、可复用性。正如前面案例中提到的，无论是TC给互联网平台付款，还是反过来互联网平台给TC付款，其本质都是对款项、财务科目和财务交易这3个业务概念的操作。
最常见到的对业务能力的误解，就是把业务能力等同于“技能”。例如，可以说面试是招聘人员的技能，但不能说面试是一个业务能力，因为面试过程中涉及到很多业务概念，以及围绕这些业务概念的很多操作（即业务能力）：

* 面试的候选人是一类员工，对应Human Resource Management能力；
* 候选人具备某种技能，对应Competency Management能力；
* 候选人面试的是某个岗位，对应Job Management能力；
* 面试是以面对面或电话会议的方式进行的，对应Conference Management能力；
* 面试前需要发送信息给候选人，告知对方面试的时间、地点等，对应Message Management能力；
* 安排面试本身是一个任务，对应Work Management能力；
* 面试过程中需要遵从一些公司内外部的政策，对应Policy Management能力；
* ……

![](http://cdn.apframework.com/11dbd74dc122eb84a5b225756c35b508.jpg)

### Value Stream（价值流）

**价值流描述企业为谁（即Stakeholder）提供什么价值，相当于企业级的业务用例。**

价值流把企业当黑盒，屏蔽了如何提供价值、有哪些场景、内部如何分工协作等细节，聚焦为谁、提供什么价值。例如，当我们分析TC有哪些价值流时，就要把TC当作一个整体（黑盒），客户希望从TC获得产品，那么客户就是Stakeholder，获得产品（Acquire Product）就是一条价值流。相对来说，“线上下单”就不是一条价值流，因为无论是线上下单还是面对面下单，客户的诉求是获得产品，如果可以不下单就获得产品，客户也会很开心。除了“获得产品”，开发产品（Develop Product）、获取原材料（Acquire Materials and Assets）、生产产品（ManufactureProduct）、优化库存（Optimize Inventory）、执行营销活动（Execute Campaign）等等都是价值流。与执行层面的业务流程相比，业务流程会随着时间的推移发生变化，但价值流非常稳定，无论是50年前还是今天，“获得产品”作为一条价值流都不存在任何变化。

一条价值流可以细分为几个价值流阶段。以“获得产品”价值流为例，可以细分为生成订单、确认订单、交付订单、客户接收产品这几个阶段，每个阶段交付明确的价值项，这些价值项汇总起来，最终交付一个完整的价值，即“获得产品”。无论客户获得产品的方式发生了多么大的变化，这条价值流以及每个价值流阶段所交付的价值项是不会改变的。

价值流的作用在于分离关注点。企业是一个运行中的复杂系统，分析复杂系统的关键在于能不能将其“切片”，即把一个复杂系统切分成若干个简单系统。价值流就是把不同Stakeholder的关注点（Concern）梳理出来，就好像把一堆杂乱无章的电线梳理成一条一条的电线，这样就方便于找出问题到底出在哪条电线上。

![](http://cdn.apframework.com/3dc4ff3eff1e2a7a896334060a234853.jpg)

看完业务能力和价值流，我们再来看看二者之间的关系。如果说价值流描述了“动态的”业务，那业务能力就是描述“静态的”业务，二者体现了业务的不同维度，它们之间没有因果关系，只有相关关系。例如，不能说“客户管理”能力是由“获得产品”价值流决定的，因为“财务结算”价值流以及其他很多价值流都会用到“客户管理”能力。

价值流和业务能力之间的相关关系体现为，在每个价值流阶段下匹配业务能力，业务能力使能价值流实现价值。以TC的“获得产品”价值流为例，客户提出需求阶段，肯定需要客户管理、产品管理能力。如果客户是通过互联网平台提出需求，则还会涉及伙伴管理能力。为及时通知客户订单处理的进度，还需要消息管理的能力。通常，价值流阶段需要匹配到L2甚至L3的业务能力，这里为了简单起见，仅仅匹配了L1的业务能力。

![](http://cdn.apframework.com/12f0ff7736d92be67390a3127a5b0d18.jpg)

看到这里，可能您已经发现，价值流与业务能力的关系与领域驱动设计中的子域与限界上下文的关系非常相似。如果从问题域和解决方案域的角度来审视，可以认为价值流体现的是问题域，即实现线上销售涉及哪几个方面的问题；而业务能力体现的是解决方案域，即哪些业务能力才能支撑价值流实现上述价值。同样，在领域驱动设计中，子域体现的是问题域，限界上下文体现的是解决方案域，那么能否认为价值流就相当于子域、业务能力就相当于限界上下文呢？

我们尝试用领域驱动设计的方式重新表述一下前面的业务架构分析结论：

* 获得产品、生产产品、财务结算三条价值流分别对应三个子域，其中获得产品子域是核心域，生产产品和财务结算是支撑域；
* 获得产品子域包含客户管理、产品管理、消息管理、伙伴管理等限界上下文；
* 生产产品子域包含订单管理、产品管理、原材料管理等限界上下文；
* 财务结算子域包含订单管理、财务管理等限界上下文。

可见，价值流对应子域，业务能力对应限界上下文，将业务架构的分析结论匹配到领域驱动设计的战略设计阶段中也是完全成立的。

![](http://cdn.apframework.com/0b19b978feb005d75408ebcef5e64482.jpg)

### Organization（组织）

组织作为业务架构四个核心元素之一，与通常意义上的组织结构类似，只是业务架构不局限于企业内部的组织结构，而是着眼于企业所处的生态，把企业内部组织和外部伙伴在一个模型上呈现出来。
组织在业务架构中的作用恰恰在于，时刻提醒我们，组织只是业务架构中的一个维度，不是全部！业务概念、价值流、业务能力等其他业务架构元素必须以独立于组织的方式描述业务，跨越组织壁垒往往是业务架构人员面临的最大挑战。在过去三年笔者从事业务架构工作的过程中，不止一次地遇到这样的场景：业务架构师向某业务部门领导汇报能力框架，领导发现某部门没有体现在业务能力框架上，于是要求业务架构师修改，最终汇报通过的业务能力框架与组织结构一一对应。不仅如此，将来一旦组织结构发生调整，业务能力框架必然要跟着调整。整个过程完美印证了“康威定律”！这样的业务能力看似每一个都有明确的业务Owner，实际上已经丧失了独立性，完全违背了业务架构所强调的“全面的、多维度的业务视角”，也不可能在战略到项目的转换过程中发挥任何作用。因此，在开发业务架构资产时，应该尽可能地避免受到组织结构的影响。

![](http://cdn.apframework.com/00a0799ab22259e0949dc03402a967c2.jpg)

### 业务架构核心元素之间的关系
用一张图总结业务架构核心元素之间的关系：

* 业务能力使用并修改业务概念
* 组织拥有业务能力
* 业务能力产出Outcome
* 业务能力使能价值流阶段
* 价值流细化为价值流阶段
* 价值流交付价值主张
* 价值流阶段交付价值项
* 价值项汇聚为价值主张
* Outcome实现价值项

![](http://cdn.apframework.com/029d6d6a64169b797597c971c47d93f4.jpg)

医生在诊断患者的病因时，脑子里一定有一套“人体结构图”，包括血液循环系统、消化系统、神经系统等等。头痛可能是因为呼吸系统感染引起的，也可能是因为神经系统出了问题。在制订解决方案前，医生必须要做出全面的评估，才能确认问题出在哪里，避免“头痛医头，脚痛医脚”。同样，企业面临各种内外部变化，要快速响应这些变化，这就必须有一套“企业结构图”，从业务概念、业务能力、价值流、组织等不同维度描述企业的业务，以及各维度之间的关联关系。相当于为物理世界中的企业在数字世界建立模型，从而帮助企业在此基础上进行变化的影响评估。这就是业务架构。

![](http://cdn.apframework.com/a807b8134e4866e420843d3b77512090.jpg)

## 业务架构原则

业务架构原则
业务架构是原则驱动的。原则是可以指导一个人的共识的真理推理。这种方法为从业人员在实践中提供了很大的自由度。建立和利用业务架构。每个主要部分都有一套原则，指导与个人蓝图和相关实践领域相关的操作。

下面列出了适用于整个业务体系结构的核心原则：

1. 业务架构与业务有关。（Business architecture is about the business.）
2. 业务架构的范围就是业务范围。（Business architecture’s scope is the scope of the business.）
3. 业务体系结构不是规定性的。（Business architecture is not prescriptive.）
4. 业务架构是迭代的。（Business architecture is iterative.）
5. 业务架构是可重用的。（Business architecture is reusable.）
6. 业务架构与可交付成果无关。（Business architecture is not about the deliverables.）

这些声明强调了基于原则的业务体系结构方法，该方法可提供从业者可以选择采用各种方法，可视化技术，工具和治理概念。共同的思路是，每种方法都遵循一套基本原则符合业务架构实践的原则，而不规定工作的方式做或限制从业者的创造力。

>These statements emphasize a principle-based approach to business architecture that provides
practitioners the option to employ a variety of methods, visualization techniques, tools, and
governance concepts. The common thread is that each approach adheres to a foundational set
of principles that aligns the practice of business architecture without dictating how the work is done or restricting the creativity of the practitioner. 

## 来源

* [中台之上：业务架构设计](https://www.infoq.cn/article/yz9lpi6W7w1EtxU4R-Mt)
* [Zachman Framework](https://en.wikipedia.org/wiki/Zachman_Framework)
* [BIZBOK业务架构知识体系介绍](http://ea.zhoujingen.cn/3346.html)
* [BIZBOK_Guide_v6.5_Part1](https://cdn.ymaws.com/www.businessarchitectureguild.org/resource/resmgr/BIZBOK_6_5/BIZBOK_Guide_v6.5_Part1.pdf)
* [业务架构——跨领域的统一语言](https://mp.weixin.qq.com/s/grrGfgitlBLfZ2jBB6mcXA)



