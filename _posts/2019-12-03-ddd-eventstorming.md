
### 事件风暴定义

EventStorming是一种灵活的研讨会格式，用于协作探索复杂的业务领域 — Alberto Brandolini （[Event Storming](https://www.eventstorming.com/)）

>EventStorming is a flexible workshop format for collaborative exploration of complex business domains.

![](http://skblog.duiduiche.com/a74556e26d65eee2638a0d65e7973072.jpg)

### 事件风暴场景

It comes in different flavours, that can be used in different scenarios:

* to assess health of an existing line of business and to discover the most effective areas for improvements;（评估现有业务运行状况和发现有效提升业务状况的措施）
* to explore the viability of a new startup business model;（探索新创业务模型的可行性）
* to envision new services, that maximise positive outcomes to every party involved;（设想新的服务，最大限度地提高每个参与方的积极成果）
* to design clean and maintainable Event-Driven software, to support rapidly evolving businesses.（帮助设计整洁可维护的事件驱动软件来支撑迅速发展的业务）

The adaptive nature of EventStorming allows sophisticated cross-discipline conversation between stakeholders with different backgrounds, delivering a new type of collaboration beyond silo and specialisation boundaries.

### 事件风暴流程

#### 物料准备

在事件风暴开始之前，需要准备以下物料：

* 便利贴：一大堆，最少要四五种不同的颜色（建议使用能够反复粘贴的便利贴）；
* 记号笔：人手一支，用于在便利贴上写写写；
* 白板：最好足够长，用来贴便利贴；
* 开放空间：用于小组成员之间的充分讨论。

![](http://skblog.duiduiche.com/732a0cbdea4d3f26461b0cb13138b401.jpg)

#### 最终示例

先有一个直观明确的目标感，最终产生的事件风暴类似如下形式

![](http://skblog.duiduiche.com/d4865faaa1ab8204d1baac6ae439b0a3.jpg)

#### 参与人员

* 组织者：组织者应当熟悉事件风暴的整个流程，能够组织大家顺利完成事件风暴；
* 领域专家：领域专家应该是精通业务的人，在事件风暴过程中，要负责澄清一些业务上的概念，思考业务上有没有遗漏的事件；
* 项目成员：负责开发这个项目的成员，所有角色都可参加，比如BA（业务架构师）、QA（质量保证）、UX（User experience design）、DEV。因为事件风暴可以快速让整个团队了解整个项目的业务流程。

#### 寻找领域事件

工作坊由寻找领域事件开始。领域事件一般用橘色的便利贴表示，书写领域实践的规则是使用被动语态，并按照时间顺序贴在白纸上。

最开始可能很多成员都不知道该怎么写，或者不知道该怎么寻找领域事件。可以由组织者写下领域中发生的第一个事件。其它参与者会迅速的开始模仿，这时我们可以让大家快速的进入状态。

在遇到有疑惑的事件时，不必长时间阻塞在那里讨论，把它作为标记记下来即可，后续再进行重点优化。可以贴一个比较醒目的便签纸（比如紫色）在事件旁边。

![](http://skblog.duiduiche.com/0c408957aee4aa8cb6feef35fc0ede32.jpg)

随着我们对业务认识的不断加深，可以随时回顾和总结之前添加的内容，对于有问题的描述进行更正，对于表述不清楚的内容可以进行重写。

事件是有相对顺序的。可以把一系列有相对顺序关系的事件放在一行上，从左到右排好。这样有助于梳理领域事件，查看是否有遗漏。

![](http://skblog.duiduiche.com/85cc89157b39bfd114b8411174c46428.jpg)

#### 寻找命令和角色

在收集完领域事件后，我们可以在此基础上进一步探索系统核心事件的运行机制。这里我们在之前的领域事件的基础上加入指令和角色的概念。

指令代表系统中用户的意图、动作和决定，一般用蓝色的便利贴表示；角色表一类特定用户，一般用黄色便利贴表示。它们之间的关系是“角色”发送“指令”产生了“领域事件”（指令也可由外部系统触发，外部系统通常用粉色的便利贴表示）。

![](http://skblog.duiduiche.com/c80058e8085688f76d35353502755fc8.jpg)

在加入指令和角色后，会触发更多关于为什么用户会产生这个操作的讨论。关于指令和角色有意思的是，指令作为用户决定的结果，当我们在思考是什么原因导致用户做这个决定时，我们会产生类似“能让用户更容易的作出决定吗？”，“能帮助用户作出更好的决定吗？”的问题。这些问题将帮助我们思考与用户做决定相关的数据模型。这时，我们会将这类数据模型记录在绿色的便利贴上。

![](http://skblog.duiduiche.com/620a55bfdd8e660fd3a186a7e850924d.jpg)

通常来说，一个命令将对应到我们后续应用程序开发的一个API。

在寻找命令和角色的过程中，你可能会遇到某些命令会在“特定的条件下”触发。比如：“当用户通过新的设备登入时，系统会发送提醒通知”。通常，我们将这种系统的行为逻辑称为策略，通常记录在紫丁香色的便利贴上，放在命令旁边。

![](http://skblog.duiduiche.com/a31e54aca9e9ebd8e51de82e02a02ec5.jpg)

#### 寻找领域模型和聚合

当我们做完了上一个环节，就可以开始寻找系统中的领域模型和聚合了。我们把跟一个概念相同的指令和事件集合到一起，并用黄色的较大的便利贴表示领域模型。

把跟这个领域模型相关的命令放到左边，事件放到右边。需要注意的是，这个时候会去掉“事件的相对顺序”这个概念，因为我们已经不需要了。

![](http://skblog.duiduiche.com/d992d961e7533db380812c3a39e39380.jpg)

可能有些领域模型不能作为一个独立存在的对象。它应该被另一个领域模型持有和使用。那这时候，可以考虑把两个模型合起来，形成一个聚合。在最上面的模型就是这个聚合的聚合根，其之下的模型都是它的实体或值对象。

现在我们有了事件，指令，角色，视图，策略和聚合的概念，它们之间的关系总结起来如下图的关系所示：

![](http://skblog.duiduiche.com/26183e7546504348246d846a7f16885f.jpg)

利用白板达到类似的效果

![](http://skblog.duiduiche.com/882e088f2d1543cac642c4ceef33a4cd.jpg)

更大规模的示例如下：

![](http://skblog.duiduiche.com/91eb5deaa10cc5cc9a5660dfc7cfeb93.jpg)



#### 划分子域和限界上下文

找到领域模型以后，我们应当就可以比较轻松地划分子域和限界上下文了。

在划分限界上下文的时候也可以反过来检验领域模型和通用语言的正确性。如果发现一个模型有歧义，那它就应该是限界上下文边界的地方，我们应该重新思考这个模型，必要时进行拆分。

### 事件风暴技巧 

在收集领域事件的过程中，由于参与方众多，一开始大家写的领域事件会比较发散，也有很多类似的描述。前期我们需要抑制内心想要统一它们的想法，因为不同的表达背后可能意味着大家不同的理解，我们可以做的是把相关的事件放在一起，一遍后续进一步分析。为了方便按照时间去组织, 我们可以在众多事件中，找到一些大家没有分歧的关键事件，然后给予关键事件来做参照，然后在关键事件标记的范围里，按照不同的组织和上下文来组织领域事件。

![](http://skblog.duiduiche.com/14ed2ae2078ea3c9ca34bfd426d483eb.jpg)


在对领域事件进行分类时，由于事件之间有不同的逻辑关系，可能需要对不同事件段内的事件进行分类。分组能让我们在排列时的逻辑更清晰，也方便我们对事件的上下文进行划分。最后，基于关键事件的领域事件分组会形成下图的结构：

![](http://skblog.duiduiche.com/7150a90bdeb538f10b3e9ac106026494.jpg)


通过时间线，我们可以更好的与众多的业务人员和领域专家进行协同，发掘领域事件。但当我们寻找聚合时，由于聚合是对业务规则的封装，保证数据的一致性，它会跨越领域事件的时间线。

参考
* 《Introducing EventStorming》
* [通过事件风暴发现业务流程 - Sarah Denayer](https://www.jdon.com/53221)
* [DDD第3篇 - 事件风暴](https://blog.csdn.net/yasinshaw/article/details/103307125)
* [事件风暴建模101](https://www.jianshu.com/p/8a7814f3e9ac)
* [事件风暴——送给奋战在前线的产品人和技术人](https://zhuanlan.zhihu.com/p/63326029)
* [Awesome EventStorming](https://github.com/mariuszgil/awesome-eventstorming)
* [A facilitators recipe for Event Storming](https://medium.com/@springdo/a-facilitators-recipe-for-event-storming-941dcb38db0d)
* [【领域驱动设计】事件风暴小体验](https://zhuanlan.zhihu.com/p/95001438)


