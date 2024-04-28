---
layout: post
title:  "架构模式：Privacy engineering"
subtitle: "InfoQ Trends Report Series"
date:   2024-04-24 8:00:00
author: "Shoukai Huang"
header-img: 'cdn.apframework.com/ff4acca9b9f4994c4bab3fec4c0c03f6.jpg'
header-mask: 0.4
tags: Architecture
---

## Privacy engineering

InfoQ Software Architecture and Design Trends Report 报告中提及的 Privacy engineering （隐私工程）

![InfoQ Software Architecture and Design Trends Report](https://cdn.apframework.com/5340918fb6d93acd5571591c121d9eae.jpg)

[InfoQ Software Architecture and Design Trends Report - April 2024](https://www.infoq.com/articles/architecture-trends-2024/)

趋势报告中这样描述：

> While security has always been an important aspect of well-architected software, we're seeing innovations under the heading of "privacy engineering". The innovation here is a shift from a reactive mindset, such as responding to GDPR, CCPA, or other regulatory requirements, to a proactive mindset where the security of users and their data is a leading design consideration. One example of this is DoorDash, which relies on personal data such as addresses and actively obfuscates or removes this data after a delivery has been made.
> 

说的是：隐私工程的创新之处在于它强调了一种从源头上保护隐私的思维模式，即通过设计和实施一系列技术和管理措施，来预防隐私泄露的风险，而不仅仅是遵守 GDPR、CCPA 等隐私保护法规的要求。这种方法要求开发者在产品或服务的设计阶段就充分考虑隐私问题，将隐私保护作为产品设计的一部分，而不是作为一项附加的责任。

### **DoorDash 的数据保护措施**

DoorDash 的例子很好地体现了隐私工程的应用。作为一个依赖用户个人数据（如地址）的配送服务平台，DoorDash 采取了主动的措施来保护用户的隐私。例如，它在完成配送后会对用户的地址等个人信息进行模糊处理或删除，这样做可以有效减少用户信息被滥用或泄露的风险。通过这种方式，DoorDash不仅提高了用户的信任度，也展示了公司对隐私保护的重视。

> 为了便于交付，用户必须向我们提供一些个人信息，包括[ 。。。]姓名、地址和电话号码[ . 。。] 。Dasher 需要这些信息才能知道将订单交付到哪里以及交付给谁。由于此信息可用于重新识别个人身份，因此不良行为者可能会利用它来造成伤害，包括身份盗用和人肉搜索。
> 
> 
> 这就是为什么我们希望确保在交付完成后的合理时间内从我们的平台编辑（删除或掩盖）这些个人数据。这样，即使不良行为者未经授权访问我们的数据库，个人数据也将不再存在，从而防止其被滥用。
> 

异步作业会触发 DoorDash 分布式系统中的数据编辑。当用户的数据符合编辑条件时，作业会将消息推送到 Kafka 主题，发出信号以清除与该用户关联的数据。保存用户数据副本的服务侦听此主题并根据请求编辑数据。从其平台上删除（或模糊）这些个人数据，以防止数据被未授权访问并滥用。

### Privacy engineering 的思考

隐私工程学作为一门新兴领域，强调从源头上保护隐私，需要对隐私进行主动的设计和实施。隐私层面上在实际业务场景中，作为重要不进急的内容，加之重要程度普及及惩罚措施不及时，重要不紧急的内容往往由于业务的紧急试错和市场抢占的时间窗口，变成了不重要不进急的事情。个人思考只有在如下几方面同时具备时，才能够很好的进行推广和普及：

1. **严格的法律保护：**法律是推动隐私工程实施的重要驱动力。例如，欧盟的GDPR和加州的CCPA等法规不仅规定了数据处理的严格标准，还设定了违反规定的严重后果，包括高额罚款。这些法律的存在迫使企业必须重视隐私保护，从而推动了隐私工程的普及。
2. **良好的云服务或基础设施提供**：随着云计算的普及，云服务提供商在隐私保护方面扮演了重要角色。他们提供的工具和服务可以帮助企业更容易地实现隐私保护措施，如数据加密、访问控制和数据泄露防护等。此外，云服务的灵活性和可扩展性也使得隐私工程的实施更加高效和经济。
3. **用户教育和意识提升**：用户需要了解他们的数据如何被收集、使用和共享，以及他们拥有哪些权利。通过教育和意识提升活动，用户可以更加明智地管理自己的数据，并要求企业采取更好的隐私保护措施。

## 附录

### General Data Protection Regulation，简称GDPR

《通用数据保护条例》（General Data Protection Regulation，简称GDPR）是欧盟（EU）在2016年通过的一项重要隐私和数据保护法规，于2018年5月25日开始实施。GDPR旨在加强和统一欧盟内所有成员国对个人数据的保护，同时给予个人更大的控制权，以监督他们的个人信息如何被使用。这项条例对在欧盟境内处理个人数据的所有组织都适用，不论这些组织是否位于欧盟境内。

GDPR的主要内容包括：

- 数据主体的权利：GDPR增强了个人对他们数据的控制权，包括访问权、被遗忘权（要求数据被删除）、数据可携带权（转移数据给其他服务提供者）等。
- 数据保护原则：数据处理应遵循特定的原则，如合法性、公平性和透明性；目的限制；数据最小化；准确性；存储限制；完整性和保密性。
- 数据保护影响评估：对于可能对个人隐私权造成高风险的数据处理活动，组织必须进行数据保护影响评估（DPIA）。
- 数据保护官（DPO）：某些组织需要指定一名数据保护官，以确保组织遵守GDPR的要求。
- 违规通报：在发生数据泄露事件时，组织必须在72小时内向监管机构报告，且在特定情况下也需通知受影响的个人。
- 跨境数据传输：GDPR对从欧盟向非欧盟国家或国际组织传输个人数据设定了严格的条件。
- 罚款和处罚：违反GDPR的组织可能面临重大的经济处罚，最高可达全球年营业额的4%或2000万欧元（以较高者为准）。

GDPR的实施对全球范围内的企业和组织产生了广泛影响，迫使它们重新评估和调整其数据保护和隐私实践。

**通用数据保护条例：** 

[通用数据保护条例 - 维基百科，自由的百科全书](https://zh.wikipedia.org/zh-cn/歐盟一般資料保護規範)

### California Consumer Privacy Act，简称CCPA

加州消费者隐私法案（California Consumer Privacy Act，简称CCPA）是美国加州于2018年通过并于2020年1月1日开始实施的一项数据保护法律。它旨在增强加州居民在数据隐私方面的权利和保护，特别是针对商业收集消费者个人信息的公司。CCPA标志着美国隐私法律的一个重要发展，是美国第一个全面的消费者隐私法律，对于在加州经营的企业及全球范围内处理加州居民数据的企业都有重大影响。

CCPA的主要规定包括：

- 知情权：企业必须在收集数据时通知消费者收集哪些信息以及如何使用这些信息。
- 访问权：消费者有权请求企业披露其收集的个人信息的具体内容及其分享给第三方的信息。
- 删除权：消费者可以要求企业删除其个人信息，企业必须执行这一请求并通知第三方也做同样的处理。
- 反对销售个人信息的权利：消费者可以指示企业不得出售其个人信息。
- 非歧视权：企业不能因为消费者行使其CCPA权利而对其进行歧视。

CCPA的实施促使许多企业重新评估其数据收集和处理实践，确保符合新的隐私标准。它也引发了美国其他州考虑或通过类似的隐私保护法律的趋势，推动了美国在数据隐私保护方面的立法进程。