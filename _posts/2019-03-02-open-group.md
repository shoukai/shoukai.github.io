---
layout: post
title:  "读书笔记：大教堂和集市"
date:   2019-03-02 08:00:00
author: "Shoukai Huang"
header-img: 'qjy1xw2zw.hn-bkt.clouddn.com/a4de0f8e499b75a0aa82603bc1cee45d.jpg'
header-mask: 0.4
tags: 读书笔记
---

![](http://qjy1xw2zw.hn-bkt.clouddn.com/33cc969c97d480c6cc60f05205d0dfcc.jpg)

## 1 大教堂和集市

### 1.1 问题域

世界上的建筑可以分两种：一种是集市，天天开放在那里，从无到有，从小到大；还有一种是大教堂，几代人呕心沥血，几十年才能建成，投入使用。

当你新建一座建筑时，你可以采用集市的模式，也可以采用大教堂的模式。一般来说，集市的特点是开放式建设、成本低、周期短、品质平庸；大教堂的特点是封闭式建设、成本高、周期长、品质优异。

Eric Raymond就问了一个问题，有没有可能用修建集市的方式，造出一所大教堂？通用，我们的问题是，有一个项目，方案A是精心准备后再投入使用，方案B是将半成品先公开，然后再逐步完善。这让我情不自禁地就想到了"大教堂和集市"这个比喻。

### 1.2 解空间

我们想造出一个大教堂，可是眼下只有一个集市，怎么办？

他说，集市要变成大教堂，有几个前提条件：

>1）你不能从零开始建设集市，你必须先有一个原始项目。（It's fairly clear that one cannot code from the ground up in bazaar style.）
>2）你的原始项目可以有缺陷，但是它必须能运行。（It can be crude, buggy, incomplete, and poorly documented. What it must not fail to do is run.）
>3）你必须向用户展示一个可行的前景，且让潜在的合作者相信在可预见的将来它会变成一个真正漂亮的东西。（When you start community-building, what you need to be able to present is a plausible promise, and convince potential co-developers that it can be evolved into something really neat in the foreseeable future.）
>4）项目的主持者本身不一定是天才，但他一定要能够慧眼识别出他人的优秀想法。（it is not critical that the coordinator be able to originate designs of exceptional brilliance, but it is absolutely critical that the coordinator be able to recognize good design ideas from others.）
>5）项目的主持者必须要有良好的人际关系、交流技能和人格魅力。这样才能吸引他人，使别人对你所做的事感兴趣，愿意帮助你。（A bazaar project coordinator or leader must have good people and communications skills.）

以上是一些必要条件，Eric Raymond也总结了一些成功的充分条件。

>1）项目首先必须是你自己感兴趣的，但是最终能对其他人有用。
>2）将用户当作合作者。
>3）尽快地和经常地做出改进，多听取用户的意见。
>4）健壮的结构远比精巧的设计来得重要。换句话说，结构是第一位的，功能是第二位的。
>5）保持项目的简单性。设计达到完美的时候，不是无法再增加东西了，而是无法再减少东西了。

开放式的文化会最终胜利，这或许不是因为"开放"在道德上正确，或者"封闭"在道德上错误，而只是因为开放式合作可以在一个问题上投入多几个数量级的技术工时，封闭的世界无法赢得这样的竞争。

### 1.3 方法课

* 作品来源：好的软件作品，往往源自于开发者的个人需要。
* 追求卓越：优秀的程序员知道写什么，卓越的程序员知道改写（和重用）什么。
* 不断尝试：在你第一次把问题解决的时候，你往往并不了解这个问题，第二次你才可能知道怎么把事情做好。所以，如果你想做对事情，至少要再做一次。
* 人员来源：如果你有正确的态度，有趣的事情自然会找到你。
* 避免包袱：当你对一个程序不再感兴趣时，你最后的责任就是把它交给一个可以胜任的接棒者。
* 工程思想：在一个已经延期的项目上增加人手，只会让项目更加延期。”更为一般地讲，Brooks定律指出，随着开发人员数目的增长，项目复杂度和沟通成本按照人数的平方增加，而工作成果只会呈线性增长。
* 拥抱用户：把你的用户当成开发合作者对待，如果想让代码质量快速提升并有效排错，这是最省心的途径。
* 发布周期：早发布，常发布，倾听用户反馈。
* 早期用户：如果你把beta测试者当做最珍贵的资源对待，他们就会成为你最珍贵的资源。
* 深耕用户：仅次于拥有好主意的是，识别来自用户的好主意，有时后者会更好。
* 不断创新：通常，那些最有突破性和最有创新力的解决方案来自于你认识到你对问题的基本观念是错的
* 保持精简：设计上的完美不是没有东西可以再加，而是没有东西可以再减



## 2 参考资料
* [《大教堂和集市》](https://book.douban.com/subject/25881855/)
* [《大教堂和集市》笔记](http://www.ruanyifeng.com/blog/2008/02/notes_on_the_cathedral_and_the_bazaar.html)
