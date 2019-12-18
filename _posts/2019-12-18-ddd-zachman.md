---
layout: post
title:  "Zachman Framework"
subtitle: "领域驱动设计系列"
date:   2019-12-18 08:00:00
author: "Shoukai Huang"
header-img: 'skblog.duiduiche.com/193a088cd5dcb077a1ee6ed37ecb86a2.jpg'
header-mask: 0.4
tags: 领域驱动
---

## Zachman Framework

Zachman框架（Zachman framework）是一种逻辑结构，它旨为信息技术企业提供一种可以理解的信息表述。它可以对企业信息按照要求分类和从不同角度进行表示。Zachman框架的创始人John Zachman早在1987年就提出了这种思想，它全称为： `企业架构和企业信息系统结构架构(Zachman Framework for Enterprise Architecture and Information Systems Architecture)。`

Zachman框架提炼和吸收了传统方法中的一些精髓，它是一款独立于信息企业所使用的工具的平台。它可以根据抽象规则定义企业信息的一个方面.一个框架采用了一种六行,每行中包含36个子单元的格式，这六行包括了范围，商业模式,系统模式，技术模式,组件和工作系统)其中有六列分别为谁，什么，什么时间，什么地点，为什么和如何做。

Zachman框架被很多企业管理者认为是一种发展IT企业和进行复杂管理的规则集合。


## Zachman Framework 概念

Zachman框架背后的基本思想是，可以使用不同类型的描述（例如，文本，图形）以不同的方式出于不同的目的描述相同的复杂事物或项目。Zachman框架提供了36种必要的类别，以完整地描述任何内容。尤其是复杂的事物，例如制成品（例如电器），建筑（例如建筑物）和企业（例如组织及其所有目标，人员和技术）。该框架从六个不同的角度提供了一个抽象概念的六种不同的转换（没有详细说明，而是进行了转换）。

它允许不同的人从不同的角度看同一件事。这将创建一个整体的环境视图，该图中显示了一项重要功能。

>The basic idea behind the Zachman Framework is that the same complex thing or item can be described for different purposes in different ways using different types of descriptions (e.g., textual, graphical). The Zachman Framework provides the thirty-six necessary categories for completely describing anything; especially complex things like manufactured goods (e.g., appliances), constructed structures (e.g., buildings), and enterprises (e.g., the organization and all of its goals, people, and technologies). The framework provides six different transformations of an abstract idea (not increasing in detail, but transforming) from six different perspectives.[24]


### Views of rows

表格中的每一行代表了在信息系统构造过程中所涉及到的某干系人在描述信息系统时所采用的视角

从特定的角度看，每一行代表解决方案的整体视图。上排或下排不一定比下排有更全面的了解。每行代表一个独特的视角。但是，每个角度的交付物都必须提供足够的详细信息以在角度级别上定义解决方案，并且必须明确转换为下一行。

每个观点都必须考虑其他观点的要求以及这些观点所施加的约束。每个视角的约束都是累加的。例如，较高行的约束会影响下面的行。较低行的约束可以但不一定影响较高行。要了解需求和约束，就需要从各个角度进行知识和理解的交流。框架指出了观点之间沟通的垂直方向。

Zachman Framework的当前版本，对行进行如下分类：

* **执行透视图（内容范围）**：第一个架构草图是 "bubble chart" 或者 Venn diagram，它以粗略形式描述了最终结构的大小、形状、部分关系和基本目的。它对应于计划者或投资人的执行摘要，该计划者或投资人希望概述或估计系统范围，其成本以及与系统运行的总体环境之间的关系。
* **业务管理透视图（业务概念）**：接下来是 architect's drawings，从所有者的角度描述了最终的架构，而所有者则必须在日常工作中与之一起生活。它们与企业（业务）模型相对应，后者构成了业务的设计，并显示了业务实体和流程以及它们之间的关系。
* **架构师视角（System Logic）**：架构师的计划是将图纸从设计师的角度转换为详细的需求表示形式。它们对应于系统分析师设计的系统模型，该系统分析师必须确定代表业务实体和流程的数据元素，逻辑流程和功能。
* **工程师视角（Technology Physics）**： The contractor 必须重新绘制建筑师的计划以代表建造者的观点，并有足够的细节来了解工具，技术和材料的约束。建造者的计划与技术模型相对应，技术模型必须使信息系统模型适应编程语言，输入/输出（I/O）设备或其他所需支持技术的详细信息。
* **技术人员视角（Tool Components）**：Subcontractors 根据车间计划工作，这些计划指定零件或小节的详细信息。这些对应于提供给程序员的详细规范，这些程序员对单个模块进行编码而无需考虑系统的整体上下文或结构。或者，它们可以代表对各种现成的商用（COTS），政府现成的（GOTS）或正在采购和实施而不是构建的模块化系统软件的组件的详细要求。
* **企业透视图或（操作实例）**：这一行的内容代表了最终产品，是架构在客观现实中的实例体现。

![](http://skblog.duiduiche.com/d7c552f92f0761a6d3046fa69b4a7327.jpg)

### Focus of columns

每个角度都将注意力集中在相同的基本问题上，然后从该角度回答这些问题，从而创建不同的描述性表示（即模型），这些表示从较高的角度转换为较低的角度。焦点（或产品抽象）的基本模型保持不变。每列的基本模型都是唯一定义的，但在整个矩阵上下都有关联。此外，企业架构组件的六类以及它们所回答的潜在疑问构成了Zachman框架的各列，分别是：

* **数据**（What，即什么内容）：用于表示客观对象的材料组成，即材料清单。对于企业来说，此部分内容就是组成事物模型（Thing Model，之所以将其称为组成事物模型而不是数据模型是因为由于不同的行代表了不同的视角，而仅在设计师所处的第三行才会关注真正信息化意义上的“数据模型”，因而在此才使用“组成事物”来对所有视角在此列中的描述对象进行指代）。
* **功能**（How，即如何工作）：用于表示功能和转换行为。对于企业来说，这部分内容就是流程或功能模型等。
* **网络**（Where，即何处）：用于表示各组成部件的坐落位置以及相互之间的联通关系。对于企业来说，这部分内容就是物流或网络模型等。
* **人**（Who，即何人负责）：用于描述了何人应该做何事，例如用户手册和操作说明等。对于企业来说，这部分内容就是人力模型或工作流模型等。
* **时间**（When，即什么时间）：用于描述事物发生的时间以及不同事物之间的相对时间关系，例如生命周期和时序图等。对于企业来说，这部分内容就是时间或动态模型等。
* **原因**（Why，即为什么做）：用于表示最终结果和意义。对于企业来说，这部分内容就是动机模型等。

在Zachman看来，使他的框架独特的唯一因素是矩阵任一轴上的每个元素都可以与该轴上的所有其他元素明确地区分开。矩阵的每个单元中的表示不仅是不断增加的详细信息的连续级别，而且实际上是不同的表示-上下文，含义，动机和用途不同。由于任一轴上的每个元素都明显不同，因此可以精确定义每个单元中的元素。

### Models of cells

Zachman框架通常被描述为有界的6 x 6“矩阵”，其中，通信疑问句为列，具体化转换为行。框架分类受单元格（即疑问句与转换之间的交集）的压制。[29]

单元描述直接取自Zachman Framework 3.0版。

![](http://skblog.duiduiche.com/65cc421d2d31154525266b116f3c2406.jpg)

**执行透视图视角**

1. （什么）材料识别 - Inventory Identification
2. （如何）过程识别 - Process Identification
3. （哪里）分布识别 - Distribution Identification
4. （谁）责任鉴定 -  Responsibility Identification
5. （何时）时间识别 - Timing Identification
6. （为什么）动机识别 - Motivation Identification

**业务管理视角**

1. （什么）材料定义 - Inventory Definition
2. （如何）过程定义 - Process Definition
3. （哪里）分布定义 - Distribution Definition
4. （谁）责任定义 - Responsibility Definition
5. （何时）时序定义 - Timing Definition
6. （为什么）动机定义 - Motivation Definition

**架构师的观点**

1. （什么）材料表示 - Inventory Representation
2. （如何）过程表示 - Process Representation
3. （哪里）分布表示 - Distribution Representation
4. （谁）责任代表 - Responsibility Representation
5. （何时）时间表示 -  Timing Representation
6. （为什么）动机表示 - Motivation Representation

**工程师视角**

1. （什么）材料规格 - Inventory Specification
2. （如何）工艺规范 - Process Specification
3. （哪里）分配规范 - Distribution Specification
4. （谁）责任规范 - Responsibility Specification
5. （何时）时序规范 - Timing Specification
6. （为什么）动机规范 - Motivation Specification

**技术员观点**

1. （什么）材料配置 - Inventory Configuration
2. （如何）流程配置 - Process Configuration
3. （哪里）分发配置 - Distribution Configuration
4. （谁）责任配置 - Responsibility Configuration
5. （何时）定时配置 - Timing Configuration
6. （为什么）动机配置 - Motivation Configuration

**企业透视图或（实际运行视角）**

1. （什么）材料实例化 - Inventory Instantiations
2. （如何）过程实例化 - Process Instantiations
3. （哪里）分布实例化 - Distribution Instantiations
4. （谁）责任实例化 - Responsibility Instantiations
5. （何时）定时实例化 - Timing Instantiations
6. （为什么）动机实例化 - Motivation Instantiations

由于每个单元中的产品开发（即体系结构工件）或该单元所体现的问题解决方案是从一个角度来看问题的答案，因此，通常，模型或描述是该单元的更高级描述或表面答案。支持该答案的精炼模型或设计是单元内的详细说明。在每个单元格内都会发生分解（即，向下钻取更多细节）。如果未将单元格设为显式（定义），则其为隐式（未定义）。如果是隐式的，则存在对这些单元格进行假设的风险。如果这些假设有效，则可以节省时间和金钱。但是，如果这些假设无效，则可能会增加成本并超出实施时间表。

### Framework set of rules（框架规则集）

该框架带有一组规则：

* 规则1，列没有顺序：列可以互换，但不能减少或创建
* 规则2，每列都有一个简单的通用模型：每列可以有自己的元模型
* 规则3，每列的基本模型必须是唯一的：每列的基本模型，关系对象及其结构都是唯一的。每个关系对象都是相互依赖的，但表示对象却是唯一的。
* 规则4，每行描述一个独特的独特视角：每行描述一个特定业务组的视图，并且对此视图是唯一的。大多数行组织中通常都存在所有行。
* 规则5，每个单元格都是唯一的：2,3＆4的组合必须产生唯一的单元格，其中每个单元格代表特定的情况。示例：A2代表业务输出，因为它们代表最终要构造的内容。
* 规则6，从该行的角度来看，所有单元格模型的组合或集成构成一个完整的模型：出于与不添加行和列相同的原因，更改名称可能会更改框架的基本逻辑结构。
* 规则7逻辑是递归的：逻辑是同一实体的两个实例之间的关系。

![](http://skblog.duiduiche.com/9abcfa32071aa38841c09d98f20058f5.jpg)

该框架是通用的，因为它可用于对任何物理对象以及概念对象（例如企业）的描述性表示进行分类。它也是递归的，因为它可用于分析自身的架构组成。尽管该框架将把关系从一列传递到另一列，但它仍然是企业的基本结构表示，而不是流程表示。

## Zachman Framework 应用和影响

综上所述，Zachman框架认为一个关于客观事物（可以是房子或飞机这种有形实体，也可以是诸如企业这样的无形概念对象）的架构描述应包括两个维度，其中，一个维度表示了对架构进行描述所应采用的六种视角，而另一个维度则代表了架构描述所需要回答的六个方面的问题。这两个维度正交交叉，从而形成了36个交汇点，其中的每一个代表了架构描述的某一具体架构制品。举例来说，不论是规划师还是设计师在描述一个系统时，都需要描述系统的数据、功能等方面，但是对于某一个具体方面，例如数据，不同的角色有着不同的理解。对于业务拥有者来说，数据指的是诸如客户、产品这样的业务实体以及他们之间的关系，而对于执行系统设计的设计师来说，数据指代的就是完全信息化意义上的数据信息片段了。除了这些明显的特性之外，Zachman框架还暗含了以下三点意义：

1. 每个架构制品仅能存在于一个表格单元中。也就是说，每个架构制品的定义都必须是清晰的，如果某个架构制品可能出现于两个或两个以上的表单元中则表示此架构制品的内容是有问题的。
2. 仅当某一个角色针对系统某一方面提供了足够的架构制品才表示与之对应的表单元是完整的，而表中所有的表单元都被填充才表示一个架构是完整的。
3. 针对表中每一列的内容必须是相互关联的。例如，在业务拥有者定义的数据必须在设计师的数据库设计中得到映射，并且每个数据库中的数据的定义都可以回溯到某个业务中的数据定义。

Zachman框架从本质上来说是对企业架构描述的一种分类法，其对于如何解决企业信息化发展所面临的问题（系统复杂度管理、业务与信息技术的不协调发展）能够提供如下的帮助：

* 给出了企业架构内容的描述和分类法，从而可以复杂的系统进行分解描述。
* 确保每个干系人的每一个关注点被照顾到。
* 改进每个架构制品使其更加契合目标受众的关注点。
* 确保业务需求可以被映射到技术实现之上，同时每个技术实现也可以被回溯到业务需求之上。
* 加强业务人员与信息技术人员的沟通和交流，减免以前因缺乏沟通而导致的无谓的内耗。

尽管如此，有些学者并不将Zachman看作为一个框架（例如，《Comparison of the Top Four Enterprise Architecture Methodologies》 的作者），而仅仅把其当成企业架构描述的一个内容分类法。这种看法是有其根据的，就其原因还是因为此框架在如下方面无法给予解答：

虽然此框架描述了企业架构应该包含哪些内容，但是并没有给出如何创建这些内容，亦即缺乏一种关于架构开发过程的描述。

在此框架之下企业架构内容就像一张静态画面一样，而企业架构是应该随着企业的发展而变化的，因而如何在不断地演进过程中对企业架构进行治理也是他缺乏的内容之一。
此框架并没有提供一个判别标准，因而无法了解按照此种方式组织的企业架构是否是一个好的架构，也就是说该框架缺乏成熟度框架。

## 参考

* [Zachman_Framework](https://en.wikipedia.org/wiki/Zachman_Framework)
* [企业架构研究总结（5）——Zachman框架](https://www.cnblogs.com/zscyun/archive/2013/05/02/3054610.html)