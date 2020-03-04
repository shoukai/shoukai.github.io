---
layout: post
title:  "Architecture 201906 摘录&搜索"
subtitle: "一些博客摘记"
date:   2019-06-30 10:45:13
author: "Shoukai Huang"
header-img: 'skblog.duiduiche.com/fb254f580a30e6d69831cf2b023f87e6.jpg'
header-mask: 0.4
tags: InfoQ
---

本摘要为阅读 Architecture 过程中的摘录及相关搜索，书籍链接：[架构师（2019年6月）](https://www.infoq.cn/article/6zDrZd2WL0f7ENQ_Ldiz)

### API（RPC或者HTTP/RESTful）设计准则

**设计准则：**

* 提供清晰的思维模型 provides a good mental model
* Make things as simple as possible, but no simpler.
* 容许多个实现 allows multiple implementations

**最佳实践:**

File API 的主要接口（以C为例，很多是 Posix API，选用比较简单的I/O接口为例）
```C
int open(const char *path, int oflag, .../*,mode_t mode */);
int close (int filedes);
int remove( const char *fname );
ssize_t write(int fildes, const void *buf, size_t nbyte);
ssize_t read(int fildes, void *buf, size_t nbyte);
```

* Document well 写详细的文档
* Carefully define the "resource" of your API 仔细的定义“资源”
* Choose the right level of abstraction 选择合适的抽象层
* Naming and identification of the resource 命名与标识
* Conceptually what are the meaningful operations on this resource? 对于该对象来说，什么操作概念上是合理的？
* For update operations, prefer idempotency whenever feasible 更新操作，尽量保持幂等性
* Compatibility 兼容
* Batch mutations 批量更新
* Be aware of the risks in full replace 警惕全体替换更新模式的风险
* Don't create your own error codes or error mechanism 不要试图创建自己的错误码和返回错误机制

来源：[深度-API 设计最佳实践的思考](https://yq.aliyun.com/articles/701810?utm_content=g_1000056442)


### RESTful API and GraphQL

RESTful API 规定了通过 GET、 POST、 PUT、 PATCH、 DELETE 等方式对服务端的资源进行操作

```T
【GET】          /users                 # 查询用户信息列表
【GET】          /users/1001            # 查看某个用户信息
【POST】         /users                 # 新建用户信息
【PUT】          /users/1001            # 更新用户信息(全部字段)
【PATCH】        /users/1001            # 更新用户信息(部分字段)
【DELETE】       /users/1001            # 删除用户信息
```

#### RESTful API 的实现分了四个层级。

* 第一层次（Level 0）的 Web API 服务只是使用 HTTP 作为传输方式。
* 第二层次（Level 1）的 Web API 服务引入了资源的概念。每个资源有对应的标识符和表达。
* 第三层次（Level 2）的 Web API 服务使用不同的 HTTP 方法来进行不同的操作，并且使用 HTTP 状态码来表示不同的结果。
* 第四层次（Level 3）的 Web API 服务使用 HATEOAS。

通常情况下，伪 RESTful API 都是基于第一层次与第二层次设计的。例如，我们的 Web API 中使用各种动词，例如 get_menu 和 save_menu ，而真正意义上的 RESTful API 需要满足第三层级以上。如果我们遵守了这套规范，我们就很可能就设计出通俗易懂的 API。

#### RESTful API 及 GraphQL 业界参考

k8s 采用了 RESTful API，链接：[kubernetes-api](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.10/)

github 部分采用了 GraphQL，链接：[GraphQL API v4](https://developer.github.com/v4/)

文档中描述了 Github 采用 GraphQL 的原因，如下

>GitHub chose GraphQL for our API v4 because it offers significantly more flexibility for our integrators. The ability to define precisely the data you want—and only the data you want—is a powerful advantage over the REST API v3 endpoints. GraphQL lets you replace multiple REST requests with a single call to fetch the data you specify.

来源：[人人都是 API 设计师：我对 RESTful API、GraphQL、RPC API 的思考](https://yq.aliyun.com/articles/702689)

### GraphQL 

官网介绍：一种用于 API 的查询语言，[GraphQL](https://graphql.cn/)

>GraphQL 既是一种用于 API 的查询语言也是一个满足你数据查询的运行时。 GraphQL 对你的 API 中的数据提供了一套易于理解的完整描述，使得客户端能够准确地获得它需要的数据，而且没有任何冗余，也让 API 更容易地随着时间推移而演进，还能用于构建强大的开发者工具。

![](http://skblog.duiduiche.com/bf05824f7cef91795d8f3e4c13ba05f6.jpg)

#### 请求你所要的数据不多不少

向你的 API 发出一个 GraphQL 请求就能准确获得你想要的数据，不多不少。 GraphQL 查询总是返回可预测的结果。使用 GraphQL 的应用可以工作得又快又稳，因为控制数据的是应用，而不是服务器。

#### 获取多个资源只用一个请求

GraphQL 查询不仅能够获得资源的属性，还能沿着资源间引用进一步查询。典型的 REST API 请求多个资源时得载入多个 URL，而 GraphQL 可以通过一次请求就获取你应用所需的所有数据。这样一来，即使是比较慢的移动网络连接下，使用 GraphQL 的应用也能表现得足够迅速。

#### 描述所有的可能类型系统

GraphQL API 基于类型和字段的方式进行组织，而非入口端点。你可以通过一个单一入口端点得到你所有的数据能力。GraphQL 使用类型来保证应用只请求可能的数据，还提供了清晰的辅助性错误信息。应用可以使用类型，而避免编写手动解析代码。

#### API 演进无需划分版本

给你的 GraphQL API 添加字段和类型而无需影响现有查询。老旧的字段可以废弃，从工具中隐藏。通过使用单一演进版本，GraphQL API 使得应用始终能够使用新的特性，并鼓励使用更加简洁、更好维护的服务端代码。

#### 使用你现有的数据和代码

GraphQL 让你的整个应用共享一套 API，而不用被限制于特定存储引擎。GraphQL 引擎已经有多种语言实现，通过 GraphQL API 能够更好利用你的现有数据和代码。你只需要为类型系统的字段编写函数，GraphQL 就能通过优化并发的方式来调用它们。

来源：[GraphQL](https://graphql.cn/)

### GraphQL 缺点

什么是 N+1 问题？首先我们来举个简单的例子：

```Groovy
def get:
    users = User.objects.all()
    for user in users:
        print(user.score)
```

这是一段简单的 python 代码，使用了 Django 的 QuerySet 来从数据库抓取数据。假设我们的数据库中有两张表 User 和 UserScore 这两张表的关系如下所示：

![](http://skblog.duiduiche.com/7a122ab5901a4b48a90542a0f571e36d.jpg)

由于用户的分数并没有保存在 User 表中，又因为 QuerySet 有 lazy load 的特性，所以在 for 循环中，每一次获取 user.score 都会查一次表，最终原本 1 次数据库查询能搞定的问题，却在不恰当的实现中产生了 N+1 次对数据库的访问。

相对于 RESTful，在 GraphQL 中更加容易引起 N+1 问题。主要是由于 GraphQL query 的逐层解析方式所引起的，关于 GraphQL 如何执行 query 的细节，可以参阅 Graphql Execution。

来源：[阻碍你使用 GraphQL 的十个问题](https://jerryzou.com/posts/10-questions-about-graphql/)

### GraphQL 应用情况

阻力：

* 现在主流的后端架构，集群间通讯还是http或者rpc的API，固定输入输出字段，根本不能满足GraphQL的需求，要上就都得改
* GraphQL的每一个实体背后可能对应着不同的数据库甚至不同类型的存储集群，后端集群间的海量数据自由join，基本还是无解的

来源：[GraphQL 为何没有火起来?](https://www.zhihu.com/question/38596306/answer/345281442)

GraphQL 确实并没有『火起来』，我觉得是这么几个因素：

* 要在前端爽爽地使用 GraphQL，必须要在服务端搭建符合 GraphQL spec 的接口，基本上是整个改写服务端暴露数据的方式。
* GraphQL 的 field resolve 如果按照 naive 的方式来写，每一个 field 都对数据库直接跑一个 query，会产生大量冗余 query，虽然网络层面的请求数被优化了，但数据库查询可能会成为性能瓶颈，这里面有很大的优化空间，但并不是那么容易做。FB 本身没有这个问题，因为他们内部数据库这一层也是抽象掉的，写 GraphQL 接口的人不需要顾虑 query 优化的问题。
* 这个事情到底由谁来做？GraphQL 的利好主要是在于前端的开发效率，但落地却需要服务端的全力配合。如果是小公司或者整个公司都是全栈，那可能可以做，但在很多前后端分工比较明确的团队里，要推动 GraphQL 还是会遇到各种协作上的阻力。这可能是没火起来的根本原因。

来源：[GraphQL 为何没有火起来?](https://www.zhihu.com/question/38596306/answer/79714979)

### 《重构:改进现有代码设计》

在《重构:改进现有代码设计》当中，Kent Beck 指出: 

>任何傻瓜都能够编写出计算机可以理解的代码，但只有优秀的程序员能够编写出人类可以理解的代码。

重构（Refactoring）就是在不改变软件现有功能的基础上，通过调整程序代码改善软件的质量、性能，使其程序的设计模式和架构更趋合理，提高软件的扩展性和维护性。

也许有人会问，为什么不在项目开始时多花些时间把设计做好，而要以后花时间来重构呢？要知道一个完 美得可以预见未来任何变化的设计，或一个灵活得可以容纳任何扩展的设计是不存在的。系统设计人员对即将着手的项目往往只能从大方向予以把控，而无法知道每 个细枝末节，其次永远不变的就是变化，提出需求的用户往往要在软件成型后，始才开始"品头论足"，系统设计人员毕竟不是先知先觉的神仙，功能的变化导致设计的调整再所难免。所以"测试为先，持续重构"作为良好开发习惯被越来越多的人所采纳，测试和重构像黄河的护堤，成为保证软件质量的法宝。

何时着手重构（Refactoring）

* 代码中存在重复的代码
* 过大的类和过长的方法
* 牵一毛而需要动全身的修改
* 类之间需要过多的通讯
* 过度耦合的信息链
* 各立山头干革命
* 不完美的设计
* 缺少必要的注释

来源：[重构（Refactoring）](https://www.cnblogs.com/guanghuiqq/archive/2012/07/23/2605261.html)
