---
layout: post
title:  "《企业IT架构转型之道》读书笔记"
subtitle: "阿里巴巴中台战略思想与架构实战"
date:   2018-01-26 8:00:00
background: 'http:/\/oo6gt25nl.bkt.clouddn.com/04.jpg'
---

# 《企业IT架构转型之道》读书笔记
-------
## 第一部分：中台战略
-------
### 第1章 阿里巴巴集团中台战略引发的思考

**中台：**

* 所谓的“中台”，并不是阿里巴巴首先提出的词语，从字面理解，中台是居于前台和后台之间。构建符合DT时代的更具创新性、灵活性的“大中台、小前台”组织机制和业务机制，即作为前台的一线业务会更敏捷、更快速适应瞬息万变的市场，而中台将集合整个集团的运营数据能力、产品技术能力，对各前台业务形成强力支撑。

### 第2章 构建业务中台的基础

**共享服务体系：**

* 符合SOA理念最核心的价值：松耦合的服务带来业务的复用，通过服务的编排助力业务的快速响应和创新。
* 共享服务体系中，人员组织角色：架构师、UED工程师、开发人员、运维工程师、DBA
* 共享服务体系中，最为核心的角色就是业务架构师，在阿里巴巴共享服务各服务中心的业务负责人一般为此角色。
* 业务架构师的能力模型正是那种典型的即懂技术，也对负责的业务领域有相当的理解。


## 第二部分：共享服务体系搭建
-------
### 第3章 分布式服务框架的选择

* 业务拆分，基于SOA理念新一代服务化框架。
    * 降低不同模块开发团队间的协同成本，业务响应更迅捷。
    * 大大降低系统间的耦合度及整体复杂度，各个开发团队可专注于各自的业务模块。
    * 避免了个别模块的错误给整体带来的影响。各个服务中心之间完全独立部署。
    * 业务拆分后解放了对单数据库集群连接数的能力依赖。每个核心服务中心有各自独立的数据库。
    * 做到针对性的业务能力扩容，减少不必要的资源浪费。

* 分布式服务框架HSF（High Speed Framework）包含以下主要组件：
    * 服务提供者
    * 服务调用者
    * 地址服务器
    * 配置服务器
    * Diamond服务器
    * 网络通信框架netty
    * 数据序列化协议Hession
    * 容错机制
    * HSF框架的线性扩展支持

### 第4章 共享服务中心建设原则

**服务中心概念**

* 共享服务中心是中台架构的基石。
* 一般来说，服务能力包括两个层次：
    * 底层PAAS能力，解决大型架构在分布式、可靠性、可用性、容错、监控以及运维层面上的通用需求；
    * 业务能力，提供云化的核心业务支撑能力，直接决定是否能真正支持上层业务做到敏捷、稳定、高效。
* 服务中心都是伴随着业务发展而变化的。所以不做过于超前的设计，也不做过于理想化的架构。
* 服务中心提供的服务能力不拘泥于接口形式，主要分为三大类：
    * 依赖于接口的服务：RPC或者是Web API
    * 依赖于工具的服务：提供定制的配置服务或者运营管理类工具
    * 依赖于数据的服务：大数据的分析能力

**服务中心的划分原则**

服务中心建设需要考量三个重要方面：设计、工程、运营；

* 架构本来就是一个追求平衡的艺术，不仅是设计原则上的平衡，还要在技术、成本、资源、性能、团队等各方面进行平衡，以最高效地解决主要问题。
* 共享服务中心的架构目的：通过业务拆分，降低系统复杂性；通过服务共享，提供可重用性；通过服务化，达到业务支持的敏捷性；通过统一的数据架构，消除数据交互的屏障。
* 服务中心建设要考量的三个重要方面：
    * 设计（面向对象的分析和设计方法）
    * 运营（完整的业务模型，要有数据运营和业务整合的价值）
    * 工程（分布式架构的优点和引入的分布式事务、问题排查等难题）
* 高内聚、低耦合原则。一个服务中心内的业务应该是相关性很高，依赖性很高的。
* 数据完整性原则。服务化架构一个很重要的业务价值就是数据模型统一。不光是业务逻辑的关键数据，还要考虑业务相关性数据，不光是实时在线数据，还要考虑离线计算的数据。
* 业务可运营性原则。我们期望服务中心是承载业务逻辑、沉淀业务数据、产生业务价值的业务单元。数据模型统一之后，可用很低成本把大数据技术引入到服务中心的架构中，让数据来源、数据分析、业务生产可以自然形成闭环。所以能否用大数据能力提升运营水平是服务中心原则之一。
* 渐进性的建设原则。如果一开始规划时应用了太多的设计原则，在实施阶段就可能会碰到拆得过细有延迟太长的问题。推荐服务化从简单开始。

### 第5章 数据拆分实现数据库能力线性扩展

* 数据库是最容易产生性能瓶颈的服务组件。

* 数据库能力扩展的工作
    * 读写分离方案：扩展了数据库的读能力，但在主数据库的数据写入能力上没法扩展。并且单表的数据量是有限的，达到一定数量后数据库性能会下滑。
    * 数据库分区方案：当单表数据量过大时，采用水平分区的方式对数据进行拆分。确保单个数据库中保存的数据量在单机数据库能提供良好的读写性能范围之内。但对于跨库的表join、事务操作、数据统计、排序等的支持变困难了。
    * 分布式数据层中间件TDDL，Taobao Distributed Data Layer.
        * 三层数据源每层都按JDBC规范实现，对前端应用没有任何代码侵入。
        * Matrix层：实现分库分表逻辑，持有多个GroupDS实例。
        * Group层：实现数据库的主备/读写分离逻辑，持有多个AtomDS实例。
        * Atom层：实现数据库连接等信息的动态推送，持有原子的数据源。

{:.center}
![IT-TDDL](http://oo6gt25nl.bkt.clouddn.com/IT-TDDL.jpg){:height="70%" width="70%" }

**数据设计原则和最佳实践：** 

* 数据尽可能平均拆分
    * 不管采用何种分库分表框架或平台，核心思路都是将原本保存在单表中太大的数据进行拆分，将这些数据分散保存到多个数据库的多个表中，避免单表过大带来的读写性能问题。
    * 如果拆分不均匀，还会产生数据访问热点，同样存在热点数据因为增长过快而又面临单表数据过大问题。
    * 数据如何拆分需要结合业务数据的结构和业务场景来决定。
    * 订单数据例子，方案一按订单ID取模，方案二按卖家用户ID取模。方案二可能会出现某些卖家的交易量非常大导致的数据不平均现象。
* 尽量减少事务边界
    * 分库分表后，如果每一条SQL语句中都能带有分库分表键，分布式服务层在解析SQL后就能精准地将该SQL推送到数据所在的数据库上执行，这是执行效率最高的方式。但不是所有业务场景在访问数据库时都能带上分库分表键，这时会出现全表扫描。
    * 事务边界：单个SQL语句，在后端数据库上同时执行的数量。
    * 全表扫描在真实的业务场景中不可避免，但如果访问不频繁，计算数据量不会太大，则不会给数据库整体性能带来太大影响。
    * 如果高并发情况下请求，为了数据库整体的扩展能力，则要考虑异构索引手段来避免这样的情况发生。
    * 如果要在内存中进行大数据量的计算，并且这类SQL频繁调用，则考虑其他平台来满足这类场景需要，比如hadoop做离线分析。如果实时要求高，则可采用内存数据库或HBase这类平台。
* 异构索引表尽量降低全表扫描频率
    * 数据异构索引的方式在实践中基本能解决和避免90%以上的跨join或全表扫描的情况，是分布式数据场景下，提升数据库服务性能和处理吞吐能力的最有效技术手段。
    * 异构索引表：采用异步机制，将原表内的每一次创建或更新，都换另一个维度保存一份完整的数据表或索引表。本质上是很多互联网公司常用的思路“用空间换时间”。
    * 应用在创建或更新一条按订单ID为分库分表键的订单数据时，也会再保存一份按照买家ID为分库分表键的订单索引数据，使得同一买家的所有订单索引表都保存在同一数据库中。
    * 数据全复制：订单按买家ID维度进行分库分表后的字段与订单ID维度分库分表的字段完全一样，就可以只进行一次数据访问了，所以建议仅仅做异构索引表，而不是数据全复制。
* 将多条件频繁查询引入搜索引擎平台
    * 某些业务场景涉及到多条件查询，如淘宝商品搜索，调用非常频繁，如果采用SQL语句方式在数据库全表扫描，必然行不通。这种场景不建议采用数据库的方式提供服务，而是采用专业的搜索引擎平台。
* 简单就是美
    * 如果“尽量减小事务边界”和“数据尽可能平均拆分”两个原则间发生了冲突，请选择“数据尽可能拆分”作为优先考虑原则。因为事务边界的问题相对好处理，无论是全表扫描或异构索引都可以。
    * 如果对每一个存在跨join或全表扫描的场景都采用异构索引的方式，数据库会出现大量冗余，数据一致性的保障也是个挑战，同时数据库间的业务逻辑关系也复杂很多，给数据库运维带来困难和风险。所以从系统风险的角度考虑，仅针对80%情况下访问的20%场景进行异构索引这样的处理，使这部分场景的性能最优。而对其他80%的场景，采用最为简单直接的方式。

### 第6章 异步化与缓存原则

* 业务流程异步化
* 数据库事务异步化
    * 将大事务拆为小事务，降低数据库的资源被长时间事务锁占用而造成的数据库瓶颈，提升平台的处理吞吐量和事务操作的响应时间。
* 数据库事务的四大属性：ACID
* CAP理论
* BASE理论
* 柔性事务如何解决分布式事务问题
    * 引入日志和补偿机制
    * 可靠消息传递
    * 实现无锁

**阿里内部成熟的分布式事务解决方案**

* 消息分布式事务。基于MQ提供的事务消息功能。
    * 通过消息进行事务异步，保证了事务的一致性，避免了两阶段提交事务方式对数据库长时间的资源锁定，所以数据库整体的吞吐和性能大大提升。
    * 本质上来说，对比柔性事务解决分布式事务的思路，消息服务扮演了事务日志的职能，事务参与者通过订阅消息建立了事务间的关联。如果出现异常，一般会采用正向补偿的方式，避免回滚。
    * 缺点是发起方要实现事务消息的发送，本地事务的执行，同时还要实现本地事务状态检查以及异常时的业务回滚机制。对开发人员提出了更高的要求。

    {:.center}
    ![mq_trans](http://oo6gt25nl.bkt.clouddn.com/mq_trans.png){:height="100%" width="100%"}

* 支付宝XTS框架。基于BASE思想，类似两阶段提交的分布式事务方案。用来保障在分布式环境下高可用性，高可靠性的同时兼顾数据一致性的要求。
    * XTS可同时支持正向和反向补偿。
    * XTS是TCC(Try/Commit/Cancel)型事务，属于典型的补偿型事务。

{:.center}
![IT-XTS](http://oo6gt25nl.bkt.clouddn.com/IT-XTS.jpg){:height="70%" width="70%"}

* AliWare TXC事务服务。
    * 标准模式下不需开发人员自行实现事务回滚或补偿，平台支持自动按事务中事务操作的顺序依次回滚和补偿。
    * Client是与Server进行交互的客户端，其中部分客户端是事务发起者，事务的创建提交由事务发起者发起。另一部分客户端则是在事务中调用的服务提供者。在业务 异常时，由TXC的客户端发起事务的回滚。
    * Server扮演了事务协调者的角色，负责对整个事务上下文的日志记录以及在事务处理过程中全局的协调和控制。
    * RM为资源管理器，一般管理多个TXC数据源，负责在TXC客户端进行数据源访问时，与TXC服务器进行事务的注册 和状态更新。
    * TXC数据源，在原有的数据源上做了一层较薄的封装，因为TXC需要拦截到所有数据修改行为，从而为事务回滚提供数据的原始值。

{:.center}
![IT-TXC](http://oo6gt25nl.bkt.clouddn.com/IT-TXC.jpg){:height="70%" width="70%"}


### 第7章 打造数字化运营能力

* 业务服务化带来的问题：
    * 各个服务间的服务调用关系变得纷繁复杂，每天海量调用，并且所有服务都是点对点交互，导致出现问题时很难定位。
* 运营服务平台建设：
    * 针对分布式服务调用链跟踪平台：鹰眼
    * 海量日志分布式处理平台：TLOG
    * 日志收集和控制

### 第8章 打造平台稳定性能力

* 技术创新和成果：
    * 限流和降级：
        * Nginx上实现的扩展组件TMD（Taobao Missile Defense）
        * Sentinel平台
    * 流量调度：HSF服务提供
    * 业务开关：业务开关管理Switch平台
    * 容量压测及评估规划。
    * 全链路压测平台。
    * 业务一致性平台。

### 第9章 共享服务中心对内和对外的协作共享

* 实现服务共享的条件：
    * 要找到一个合适的服务化对象
    * 建设对象服务化的基础设施
    * 服务化实施阶段
    * 增强服务和基础设施实现服务的精细治理
* 建设思路：
    * 确定服务化的对象是API
    * 建立共享服务的基础设施，实现API的服务封装
    * 服务化实施阶段：
        * API as Service
        * Product as Service
        * Solution as Service

## 第三部分：阿里巴巴能力输出与案例
-------
略

### 参考
-------
* [事务消息](https://help.aliyun.com/document_detail/43348.html)
* [《企业IT架构转型之道》读书笔记](https://zhuanlan.zhihu.com/p/29936037)





