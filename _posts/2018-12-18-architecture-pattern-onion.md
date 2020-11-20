---
layout: post
title:  "架构模式：Onion Architecture"
date:   2018-11-01 8:00:00
author: "Shoukai Huang"
header-img: 'cdn.apframework.com/f4d6469ceef0c79e8615cd6e722a7770.jpg'
header-mask: 0.4
tags: 软件架构
---

# Onion Architecture

## 架构介绍

在洋葱的层之间，存在强依赖性规则：外层可以依赖于较低层，但是较低层中的代码不能直接依赖于外层中的任何代码。这本质上是依赖性倒置原则，只是根据整体架构而不仅仅是单个类来呈现。

洋葱架构的本质是依赖倒置原则的应用，以及层之间的架构定义优先级。杰弗里·巴勒莫（Jeffrey Palermo）在他关于洋葱建筑的原始文章中强调了这一点 - 洋葱建筑与分层建筑之间的主要区别在于依赖关系的方向（以及耦合）。

架构来源：

* [The Onion Architecture : part 1](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/)
* [The Onion Architecture : part 2](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-2/)
* [The Onion Architecture : part 3 ](https://jeffreypalermo.com/2008/08/the-onion-architecture-part-3/)
* [Onion Architecture: Part 4 – After Four Years](https://jeffreypalermo.com/2013/08/onion-architecture-part-4-after-four-years/)


**主要原则**：

* 该应用程序围绕独立的对象模型构建
* 内层定义接口。外层实现接口
* 耦合方向朝向中心
* 所有应用程序核心代码都可以编译并与基础架构分开运行

**架构层次**

![Onion Architecture](http://cdn.apframework.com/Onion Architecture core.png)

1. 核心（Core）层：是与领域或技术无关的基础构件块，它包含了一些通用的构件块，例如 list、case 类或 Actor 等等。核心层不包含任何技术层面的概念，例如 REST 或数据库等等。
2. 领域（Domain）层：是定义业务逻辑的地方，每个类的方法都是按照领域通用语言中的概念进行命名的。对领域层的控制是通过 API 层进行操作的，而所有的业务逻辑都归属于领域层。这种方式保证了应用程序的可移植性，在不丢失任何业务逻辑的情况下替换掉整个技术实现。
3. API层：是领域层的入口，它使用领域中的术语和对象。Wade 提到：API 层应该仅仅向外界暴露不可变的对象，以避免开发者通过暴露的对象获得对底层领域的访问，并任意修改领域行为。Wade 通常会从 API 层开始编码工作，每个方法就是一个骨架，并且对应一个高层次的功能性测试。随后添加代码逻辑以使该测试通过，以此驱动领域层的编码实现。
4. 基础架构（Infrastructure）层：是最外部的一层，它包含了对接各种技术的适配器，例如数据库、用户界面以及外部服务。它能够访问所有处于内部的层次，但多数操作是通过 API 层进行的。但也有一种例外情况的存在 ，就是负责实现领域层中所定义的某些接口（译注：例如各种 Repository 的接口）。

**优点**

1. DDD友好：对于在域模型之上构建所有内容的架构而言，可以很好地发挥作用
2. 定向耦合：我们应用程序中最重要的代码取决于什么，一切都取决于它
3. 灵活性：从内层角度来看，您可以在任何外层交换任何东西，事情应该可以正常工作
4. 可测试性：由于您的应用程序核心不依赖于任何其他内容，因此可以单独轻松快速地进行测试

**不足**

1. 学习曲线：人们倾向于在层之间分散责任
2. 间接：到处都是接口！
3. 可能很重：示例中使用了Hibernate

**与六边形架构的区分**

* 洋葱架构有时也被称为端口和适配器（Ports and Adapters）架构，或者是六边形（Hexagonal）架构。有时也认为，后者应该是洋葱架构的一个超集。
* 六边形体系结构和洋葱架构共享以下前提：外部化基础架构和编写适配器代码，以便基础架构不会紧密耦合。

**示例**

* [Onion Architecture](https://bitbucket.org/jeffreypalermo/onion-architecture)
* [codecampserver](https://archive.codeplex.com/?p=codecampserver)

## 参考

* [在洋葱（Onion）架构中实现领域驱动设计](https://www.infoq.cn/article/2014%2F11%2Fddd-onion-architecture)
* [洋葱架构(译)](https://www.jianshu.com/p/d87d5389c92a)
* [Onion Architecture Is Interesting](https://dzone.com/articles/onion-architecture-is-interesting)

