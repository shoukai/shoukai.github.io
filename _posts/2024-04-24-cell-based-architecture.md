---
layout: post
title:  "架构模式：Cell-Based Architecture"
subtitle: "InfoQ Trends Report Series"
date:   2024-04-24 8:00:00
author: "Shoukai Huang"
header-img: 'cdn.apframework.com/3c216876873204be5e075aae3ba25df2.jpg'
header-mask: 0.4
tags: Architecture
---

阅读 InfoQ Software Architecture and Design Trends Report 报告中，看到有单元化架构（Cell-Based Architecture）趋势提及。

![InfoQ Software Architecture and Design Trends Report](https://cdn.apframework.com/5340918fb6d93acd5571591c121d9eae.jpg)

[InfoQ Software Architecture and Design Trends Report - April 2024](https://www.infoq.com/articles/architecture-trends-2024/)

趋势报告中这样描述

> Microservice architectures are deployed with services using an "everyone talks to everyone" approach. Cell-based architecture imposes routing constraints where services prefer calling other services in the same cell, which can often be an availability zone. This can lead to significant cost savings while also increasing performance due to reduced latency. When implemented correctly, cell-based architectures increase availability since faults are contained in one cell, leaving other cells fully functioning.
> 

说的是：服务之间采用“everyone talks to everyone"的方法。单元化架构施加了路由约束，其中服务更倾向于调用同一单元内的其他服务，这通常可以是一个可用区。这可以带来显著的成本节约，同时由于减少了延迟，也提高了性能。如果正确实施，单元化架构会增加可用性，因为故障被限制在一个单元内，使其他单元能够完全正常运行。

介绍比较简略，查阅了一些资料，对单元化架构进行了一些整理。架构模式有适用的场景和所需要解决的问题，个人理解更适用于需要稳定性要求较高、规模较大的复杂系统场景，当然云服务商的基础设施完善时，中小规模系统也可以进行落地尝试。

## 单元化架构（Cell-Based Architecture）

### 1. 问题起源

单元化架构起源于对可扩展和弹性系统的需求，特别是在数字服务领域。随着技术的发展，企业和服务平台需要能够快速扩展以应对流量激增的情况，同时保持服务的连续性和可靠性。这种架构方法对于快速扩展以响应不断变化的需求至关重要，已成为数字化成功的基础。

### 2. 概念范围

单元化架构是一种将大型复杂系统分解为更小、独立、自包含单元的方法。每个单元都是一个完整的系统的一部分，拥有自己的数据存储、计算能力和应用逻辑。类似于城市中社区的运作方式。它们独立运行，因此如果一个单元出现问题，不会影响系统的其他部分。

这种模块化的设置使得每个单元可以独立地进行扩展、部署和管理，从而增强了系统的整体可扩展性和故障恢复能力。单元化架构借鉴了分布式系统和微服务设计模式的原则。

![UntCell-Based Architecturetled](https://cdn.apframework.com/e786424c137768bcf631e6eff6675219.jpg)

### 3. 要解决的问题

单元化架构旨在解决以下问题：

- 可扩展性：在面对不断增长的用户数量和数据处理需求时，单元化架构允许系统通过增加更多的单元来水平扩展，而不是仅仅通过增加单个单元的大小（垂直扩展）。
- 弹性：单元化架构通过将系统分割成独立的单元，使得每个单元可以独立于其他单元运行和恢复，从而提高了整个系统的弹性和故障恢复速度。
- 系统复杂性：随着业务增长，传统架构变得难以管理和维护。单元化架构通过模块化设计，简化了系统的管理和维护工作。
- 独立部署：单元化架构允许对系统的特定部分进行独立的部署和更新，而不会影响到其他部分，这样可以减少部署新功能时的风险。

### 4. 适用的场景

单元化架构是一种特别适用于以下场景和需求的系统设计策略：

- 高风险应用：对于那些不能承受长时间停机的应用，如在线交易平台或实时通信服务，使用单元化架构可以减少系统故障对客户的影响，保护企业声誉，并避免重大的财务损失。
- 关键经济基础设施：对于金融服务行业等关键经济领域，系统必须持续运行以维持经济稳定。单元化架构通过隔离故障和快速恢复，确保了金融服务的高可用性。
- 超大规模系统：对于规模庞大、关键到不能容忍任何故障的系统，单元化架构提供了一种有效的解决方案，允许系统在面对故障时依然能够维持运行。
- 严格的恢复目标：对于需要极短恢复时间目标（RTO）和恢复点目标（RPO）的应用，单元化架构可以快速恢复服务，最小化故障对业务的影响。
- 具有专用需求的多租户服务：在多租户架构中，如果租户需要专用资源以满足服务水平协议（SLA）的要

### 5. 解决问题思路

设计和实现基于单元的架构时发挥作用的关键设计因素。

- 单元设计：确定功能边界，根据业务需求，确定系统中可以独立运作的功能，并将其作为单独的单元。资源分配，为每个单元配备必要的资源，如数据库、存储和计算资源，确保单元的独立运行。
- 单元间通信：API网关，使用API网关来管理单元间的通信，提供统一的接口和版本控制。消息队列，对于异步通信，使用消息队列来解耦单元，提高系统的弹性。
- 数据管理：数据隔离，每个单元应该有自己的数据存储，避免数据依赖和共享状态。数据一致性，对于需要跨单元共享的数据，实现如事件溯源、CQRS（命令查询分离）等数据一致性模式。

### 6. 所面临的问题

实现基于单元的架构确实带来了一系列挑战，需要仔细考虑和解决，而且每一个都容易解决。

1. 架构复杂性：设计和实施单元化架构需要对分布式系统有深入的理解，包括单元边界的定义、数据分区策略和单元间通信协议的制定。
2. 资源和基础设施开销：每个单元可能需要独立的资源，如服务器、存储和网络资源，这可能导致比共享资源模型更高的成本。
3. 单元间通信管理：设计高效的通信机制，以确保单元间的交互既一致又高效，同时避免紧密耦合和性能瓶颈。
4. 数据一致性和同步：在单元化架构中维护数据一致性是一个挑战，可能需要采用复杂的策略，如事件溯源、CQRS或分布式事务。
5. 专业工具和实践：需要专业的工具和实践来管理多个工作负载实例，这可能涉及到对现有工具链的扩展或替换。
6. 路由层建设：为了正确引导流量，可能需要在单元路由层上进行额外的技术储备和建设。

总的来说，架构会更加复杂，增加研发成本的同时，需要突破数据一致性和路由等基础设施的技术难题。

### 7. 目前应用案例

应用案例：亚马逊（Amazon）Facebook、DoorDash、Slack、Roblox、Netflix、谷歌（Google）。架构策略有利于动态扩展和强大的故障隔离，并需要仔细考虑增加的复杂性、资源分配以及对专门操作工具的需求。


### 8. 参考资料

1. [Cell-Based Architecture: Comprehensive Guide - DZone](https://dzone.com/articles/grokking-cell-based-architecture)
2. [Cell-Based Architecture](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md)
3. [InfoQ Software Architecture and Design Trends Report - April 2024](https://www.infoq.com/articles/architecture-trends-2024/)

