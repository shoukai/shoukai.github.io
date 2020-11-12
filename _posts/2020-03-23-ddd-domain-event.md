---
layout: post
title:  "DDD：Domain event"
subtitle: "领域驱动设计系列"
date:   2020-03-23 08:00:00
author: "Shoukai Huang"
header-img: 'qjy1xw2zw.hn-bkt.clouddn.com/279b378ccfbf0926d0cf2bf5741b29ea.jpg'
header-mask: 0.4
tags: 领域驱动
---


## Domain event 定义

### Eric Evans 定义

Eric Evans 《领域驱动设计》作者

> A full-fledged part of the Domain Model, a representation of something that happened in the domain. Ignore irrelevant domain activity while making explicit the events that the domain Experts want to track or be notified of, or which are associated with state change in the other Model objects.

域模型的完整部分，表示域中发生的事情。忽略无关的域活动，同时明确显示域专家要跟踪或被通知的事件，或与其他Model对象中的状态更改关联的事件。

### Vaughn Vernon 定义

Vaughn Vernon 《实现领域驱动设计》作者

> An occurrence of something that happened in the domain.

### Martin Fowler 定义

Martin Fowler 《重构－改善既有代码的设计》、《UML精粹：标准对象建模语言简明指南》、《分析模式：可重用的对象模型》、《企业应用架构模式》……作者

> Captures the memory of something interesting which affects the domain.

## Domain event 解释

### Chris Richardson 解释

来自 Chris Richardson 相关解释，by：[Pattern: Domain event](https://microservices.io/patterns/data/domain-event.html) 

内容：

>A service often needs to publish events when it updates its data. These events might be needed, for example, to update a CQRS view. Alternatively, the service might participate in an choreography-based saga, which uses events for coordination.

问题域：

>How does a service publish an event when it updates its data?

解空间：

>Organize the business logic of a service as a collection of DDD aggregates that emit domain events when they created or updated. The service publishes these domain events so that they can be consumed by other services.

如上所述，领域事件用在领域聚会创建和更新时，服务发送领域事件，其他订阅服务接收领域事件。

### 微软文档相关解释

来自微软相关解释，by：[Domain events: design and implementation](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation)

> An event is something that has happened in the past. A domain event is, something that happened in the domain that you want other parts of the same domain (in-process) to be aware of. The notified parts usually react somehow to the events.

领域事件是同一领域多个聚合被触发的首选方式

> Domain events as a preferred way to trigger side effects across multiple aggregates within the same domain

![](http://qjy1xw2zw.hn-bkt.clouddn.com/711026ada4e13c588a2fe83075f0901d.jpg)

Domain events to enforce consistency between multiple aggregates within the same domain

领域事件使同一领域不同聚合增加了一致性

## Domain event 事务

有时讨论的是在同一个事务中发生的（还未持续化）。有时讨论的event是已经持续化之后才发生的事件，因为要传播到其他的微服务，限定上下文或者外部系统使其保持一致性。后者是已经发生过了的，甚至已经持续化后的。

领域内 Domain Events，在 in-process (in-memory) 同一个事务中完成所有的操作。在领域实体的业务方法中发布事件，在多个handler中处理这个事件，但是必须是同一事务。Domain Events 作为一种模式不总是适用的。但是 in-memory event 保证了同一个上下文和微服务中领域模型的一致性。

领域外 Domain Events，或者称之为领域外的事件通知，依托消息中间件进行数据传递，依托分布式事务相关模式保障数据一致性。

![](http://qjy1xw2zw.hn-bkt.clouddn.com/88d2dc2d0ccf1d20f14b03fc1f48c740.jpg)


## Domain event in Spring

Spring Data 在很多地方都是按照DDD原则进行的设计(如 Repository), 这里 Spring Data 主要是实现了 DDD 的aggregate 和 domain event:

### @TransactionalEventListener

使用 @TransactionalEventListener 监听，使用 TransactionPhase.AFTER_COMMIT 可在事务完成后，再执行事件监听方法，从而保证数据的一致性

* TransactionPhase.BEFORE_COMMIT 事务提交前
* TransactionPhase.AFTER_COMMIT 事务提交后
* TransactionPhase.AFTER_ROLLBACK 事务回滚后
* TransactionPhase.AFTER_COMPLETION 事务完成后

```JAVA
@Component
public class GenderStatProcessor {
    @Autowired
    GenderRepository genderRepository;

    @Async
    @TransactionalEventListener
    public void handleAfterPersonSavedComplete(PersonSavedEvent event){
        GenderStat genderStat = genderRepository.findOne(1l);
        if(event.getGender()==1){
            genderStat.setMaleCount(genderStat.getMaleCount()+1);
        }else {
            genderStat.setFemaleCount(genderStat.getFemaleCount()+1);
        }
        genderRepository.save(genderStat);
    }
}
```

### @DomainEvents

@DomainEvents作用在实体上的

```JAVA
@Entity
@Data
@AllArgsConstructor
@NoArgsConstructor
@ToString(exclude = "domainEvents")
public class Person  {//extends AbstractAggregateRoot{
    @Id
    @GeneratedValue
    private Long id;
    private String name;
    private Integer gender;//1:male;2:female

    //该方法会在 PersonRepository save() 调用时被触发调用
    @DomainEvents
    Collection<Object> domainEvents() {
        List<Object> events= new ArrayList<Object>();
        events.add(new PersonSavedEvent(this.id,this.gender));
        return events;
    }
}
```

### @Async

没有此注解事件监听方法与主方法为一个事务。

### 代码

代码：[spring-data-domain-event](https://github.com/shoukai/tools-journey/tree/master/spring-data-domain-event)

调用接口，存储 Person

```SHELL
curl --location --request POST 'http://localhost:8080/people' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name":"hello",
    "gender":1
}'
```

存储 Persion 的时候，发送领域事件， GenderStatProcessor 监听领域事件并进行事件处理

## 参考

* [Pattern: Domain event](https://microservices.io/patterns/data/domain-event.html)
* [Domain events: design and implementation](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation)
* [[译]DDD和微服务中Domain Events vs. Integration Events](http://www.ichub.com/portal/article/index/id/1623/cid/34.html)
* [Domain Event pattern](https://badia-kharroubi.gitbooks.io/microservices-architecture/content/patterns/tactical-patterns/domain-event-pattern.html)
* [spring-data-domain-event](https://github.com/wiselyman/spring-data-domain-event)
* [基于Spring Data的领域事件发布](https://segmentfault.com/a/1190000022237108)




