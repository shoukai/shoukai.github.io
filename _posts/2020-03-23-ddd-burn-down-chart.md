---
layout: post
title:  "DDD：燃尽图"
subtitle: "领域驱动设计系列"
date:   2020-03-23 08:00:00
author: "Shoukai Huang"
header-img: 'skblog.duiduiche.com/279b378ccfbf0926d0cf2bf5741b29ea.jpg'
header-mask: 0.4
tags: 领域驱动
---


## 定义

燃尽图（英语：burn down chart）：用于表示剩余工作量的工作图表，由横轴（X）和纵轴（Y）组成，横轴表示时间，纵轴表示工作量。这种图表可以直观的预测何时工作将全部完成，常用于软件开发中的敏捷软件开发方式，也可以用于其他类型的工作流程监控。

一个完整的燃尽图。一般来说，燃尽图可按照下列内容来理解：

![](http://skblog.duiduiche.com/acb26ea73535be3715417fa120788513.jpg)

* X-Axis：The project/iteration timeline（项目/迭代时间表）
* Y-Axis：The work that needs to be completed for the project. The time or story point estimates for the work remaining will be represented by this axis.（该项目需要完成的工作。剩余作品的时间或故事点估计将由此轴表示。）
* Project Start Point：This is the farthest point to the left of the chart and occurs at day 0 of the project/iteration.（这是图表左侧最远的点，发生在项目/迭代的第0天。）
* Project End Point	：This is the point that is farthest to the right of the chart and occurs on the predicted last day of the project/iteration（这是图表右侧最远的一点，发生在项目/迭代的预计最后一天）
* Number of Workers and Efficiency Factor：In the above example, there are an estimated 28 days of work to be done, and there are two developers working on the project, who work at an efficiency of 70%. Therefore, the work should be completed in (28 ÷ 2) ÷ 0.7 = 20 days.（在上面的示例中，估计需要完成28天的工作，并且有两个开发人员在该项目上工作，其工作效率为70％。因此，应在（28÷2）÷0.7 = 20天内完成工作。）
* Ideal Work Remaining Line：This is a straight line that connects the start point to the end point. At the start point, the ideal line shows the sum of the estimates for all the tasks (work) that needs to be completed. At the end point, the ideal line intercepts the x-axis showing that there is no work left to be completed. Some people take issue with calling this an "ideal" line, as it's not generally true that the goal is to follow this line. This line is a mathematical calculation based on estimates, and the estimates are more likely to be in error than the work. The goal of a burn down chart is to display the progress toward completion and give an estimate on the likelihood of timely completion.（这是一条连接起点和终点的直线。在起点，理想线显示了需要完成的所有任务（工作）的估算值之和。在终点，理想线与x轴相交，表明没有工作要做。有些人称其为“理想”行会产生问题，因为通常目标并非遵循这一行。这条线是基于估计的数学计算，并且比工作更容易出错。燃尽图的目标是显示完成进度，并估计及时完成的可能性。）
* Actual Work Remaining Line：This shows the actual work remaining. At the start point, the actual work remaining is the same as the ideal work remaining but as time progresses, the actual work line fluctuates above and below the ideal line depending on this disparity between estimates and how effective the team is. In general, a new point is added to this line each day of the project. Each day, the sum of the time or story point estimates for work that was recently completed is subtracted from the last point in the line to determine the next point.（这显示了实际剩余工作。在开始时，实际剩余工作量与理想剩余工作量相同，但是随着时间的推移，实际工作线会在理想值上下波动，具体取决于估算值与团队效率之间的差异。通常，项目的每一天都会向该行添加一个新点。每天，将从行中的最后一点减去最近完成的工作的时间或故事点估计的总和，以确定下一点。）


## 应用

燃尽图可以用在Sprint中，也可以用在Epic中。

![](http://skblog.duiduiche.com/c5685ae301083376d7b1f77e8d35a74b.jpg)

这个表中我们可以看到：

* 有多少工作已经完成？
* 有多少工作需要完成？
* 每天都干了多少价值的工作？
* 当下的工作速度是否跟得上计划？
* ……

很多时候，会发现和想象的不一样，比如：新的需求义无反顾的来了，理想太丰满现实完不成工期了，再或者在老板的重压下团队效率大爆发了……

![](http://skblog.duiduiche.com/be93781d2cbe13d5ca87c0027835d515.jpg)

可以看出，实际项目中有时会添加任务，为了满足项目周期，可以删除部分任务也可以添加人员，满足项目周期需求。

## 例子

优秀团队的燃尽图，不需要干预

![](http://skblog.duiduiche.com/938803eea95394175f3e3e3d9b139286.jpg)

deadline 驱动团队的燃尽图，满足里程碑任务，但是最后需要付出努力

![](http://skblog.duiduiche.com/347a21fb31bc94275a269c919e9e7b06.jpg)

太快了团队的燃尽图，任务估算不准确

![](http://skblog.duiduiche.com/d27f916a5ba029336a113222f1b610d6.jpg)

太迟了团队的燃尽图，任务难度或者工作量估算不足

![](http://skblog.duiduiche.com/da65c63032810c9f9ba5baa0914bcc72.jpg)

形式化的燃尽图（未真正使用），检查的时候更新一下

![](http://skblog.duiduiche.com/268f1121327febcf89436abb5726eb28.jpg)

上天团队的燃尽图，任务数量或者工作内容估算不足

![](http://skblog.duiduiche.com/d8629464ab9ae9310c47d182d61c0664.jpg)



## 参考

* [Burn down chart](https://en.wikipedia.org/wiki/Burn_down_chart)
* [从老板到项目成员，如何从燃尽图中洞悉团队工作？](http://www.woshipm.com/chuangye/2432620.html)













