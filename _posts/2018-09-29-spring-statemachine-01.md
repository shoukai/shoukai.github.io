---
layout: post
title:  "开源软件：Spring Statemachine 01：概念及应用"
subtitle: "Spring Statemachine is a framework for application developers to use state machine concepts with Spring applications."
date:   2018-09-29 8:00:00
author: "Shoukai Huang"
header-img: 'cdn.apframework.com/f4d6469ceef0c79e8615cd6e722a7770.jpg'
header-mask: 0.4
tags: 状态机 开源软件
---

# Spring Statemachine 概念及应用

## 1 Finite-state machine

### 1.1 状态机定义
有限状态机，（英语：Finite-state machine, FSM），又称有限状态自动机，简称状态机，是表示有限个状态以及在这些状态之间的转移和动作等行为的数学模型。

有限状态机体现了两点：首先是离散的，然后是有限的。

* State：状态这个词有些难以定义，状态存储关于过去的信息，就是说它反映从系统开始到现在时刻的输入变化。
* Transitions：转换指示状态变更，并且用必须满足来确使转移发生的条件来描述它。
* Actions：动作是在给定时刻要进行的活动的描述。
* Guards：检测器出现的原因是为了检测是否满足从一个状态切换到另外一个状态的条件。
* Event：事件，又见事件，笼统说来，对系统重要的某件事情被称为事件。

### 1.2 状态机示例

现实中的例子：验票闸门，来自wiki

![Turnstile.alewife.agr](http://cdn.apframework.com/Turnstile.alewife.agr.jpg){:height="100%" width="100%" }

用于控制地铁和游乐园游乐设施的旋转门是一个门，在腰高处有三个旋转臂，一个横跨入口通道。最初，手臂被锁定，阻挡了入口，阻止了顾客通过。将硬币或代币存放在旋转门上的槽中可解锁手臂，允许单个客户穿过。在顾客通过之后，再次锁定臂直到插入另一枚硬币。

旋转门被视为状态机，有两种可能的状态：锁定和解锁。有两种可能影响其状态的输入：将硬币放入槽（硬币）并推动手臂（推动）。在锁定状态下，推动手臂无效; 无论输入推送次数多少，它都处于锁定状态。投入硬币 - 即给机器输入硬币 - 将状态从锁定转换为解锁。在解锁状态下，放入额外的硬币无效; 也就是说，给予额外的硬币输入不会改变状态。然而，顾客推动手臂，进行推动输入，将状态转回Locked。

旋转门状态机可由状态转换表表示，显示每个可能状态，它们之间的转换（基于给予机器的输入）和每个输入产生的输出：

| Current State | Input | Next State | Output |
| --- | --- | :-: | :-: |
| Locked | coin | Unlocked | 解锁旋转门，以便游客能够通过 |
| Locked | push | Locked | None |
| Unlocked | coin | Unlocked | None |
| Unlocked | push | Locked | 当游客通过，锁定旋转门 |

旋转栅状态机也可以由称为状态图的有向图表示 （上面）。每个状态由节点（圆圈）表示。边（箭头）显示从一个状态到另一个状态的转换。每个箭头都标有触发该转换的输入。不引起状态改变的输入（例如处于未锁定状态的硬币输入）由返回到原始状态的圆形箭头表示。从黑点进入Locked节点的箭头表示它是初始状态。

![790px-Turnstile_state_machine_colored.svg](http://cdn.apframework.com/790px-Turnstile_state_machine_colored.svg.png){:height="100%" width="100%" }


## 2 Spring Statemachine

### 2.1 定位及特色
Spring Statemachine is a framework for application developers to use state machine concepts with Spring applications. Spring Statemachine 是应用程序开发人员在Spring应用程序中使用状态机概念的框架。

Spring Statemachine 提供如下特色：

* Easy to use flat one level state machine for simple use cases.（易于使用的扁平单级状态机，用于简单的使用案例。）
* Hierarchical state machine structure to ease complex state configuration.（分层状态机结构，以简化复杂的状态配置。）
* State machine regions to provide even more complex state configurations.（状态机区域提供更复杂的状态配置。）
* Usage of triggers, transitions, guards and actions.（使用触发器、transitions、guards和actions。）
* Type safe configuration adapter.（应用安全的配置适配器。）
* Builder pattern for easy instantiation for use outside of Spring Application context（用于在Spring Application上下文之外使用的简单实例化的生成器模式）
* Recipes for usual use cases（通常用例的手册）
* Distributed state machine based on a Zookeeper State machine event listeners.（基于Zookeeper的分布式状态机状态机事件监听器。）
* UML Eclipse Papyrus modeling.（UML Eclipse Papyrus 建模）
* Store machine config in a persistent storage.（存储状态机配置到持久层）
* Spring IOC integration to associate beans with a state machine.（Spring IOC集成将bean与状态机关联起来）

### 2.2 发展及社区

相关资源：

* Spring Statemachine 主页：[Home](https://projects.spring.io/spring-statemachine/)
* Spring Statemachine Github：[Github](https://github.com/spring-projects/spring-statemachine)，Star：500+ & Fork：200+ （201809）
* Spring Statemachine Gitter：[Gitter](https://gitter.im/spring-projects/spring-statemachine)， less than 100 people

发布版本：

最新版本：[v2.0.2.RELEASE](https://github.com/spring-projects/spring-statemachine/releases/tag/v2.0.2.RELEASE) 及 [v1.2.12.RELEASE](https://github.com/spring-projects/spring-statemachine/releases/tag/v1.2.12.RELEASE) 已经44次版本发布，更详尽和最新情况请查看Github主页。


# 3 功能示例

## 3.1 基本功能

继续旋转门的现实例子，添加Maven依赖，本例子中使用版本如下：

```
<dependency>
    <groupId>org.springframework.statemachine</groupId>
    <artifactId>spring-statemachine-core</artifactId>
    <version>2.0.2.RELEASE</version>
</dependency>
```

定义旋转门所处状态：锁定、解锁，使用枚举

```
public enum TurnstileStates {
    Unlocked, Locked
}
```

定义旋转门操作事件：推门和投币，使用枚举

```
public enum TurnstileEvents {
    COIN, PUSH
}
```

状态机配置，其中turnstileUnlock()和customerPassAndLock()即为当前状态变更后的扩展业务操作，可以根据实际业务场景进行修改

```
@Configuration
@EnableStateMachine
public class StatemachineConfigurer extends EnumStateMachineConfigurerAdapter<TurnstileStates, TurnstileEvents> {

    @Override
    public void configure(StateMachineStateConfigurer<TurnstileStates, TurnstileEvents> states)
            throws Exception {
        states
                .withStates()
                // 初识状态：Locked
                .initial(TurnstileStates.Locked)
                .states(EnumSet.allOf(TurnstileStates.class));
    }

    @Override
    public void configure(StateMachineTransitionConfigurer<TurnstileStates, TurnstileEvents> transitions)
            throws Exception {
        transitions
                .withExternal()
                .source(TurnstileStates.Unlocked).target(TurnstileStates.Locked)
                .event(TurnstileEvents.COIN).action(customerPassAndLock())
                .and()
                .withExternal()
                .source(TurnstileStates.Locked).target(TurnstileStates.Unlocked)
                .event(TurnstileEvents.PUSH).action(turnstileUnlock())
        ;
    }

    @Override
    public void configure(StateMachineConfigurationConfigurer<TurnstileStates, TurnstileEvents> config)
            throws Exception {
        config.withConfiguration()
                .machineId("turnstileStateMachine")
        ;
    }

    public Action<TurnstileStates, TurnstileEvents> turnstileUnlock() {
        return context -> System.out.println("解锁旋转门，以便游客能够通过" );
    }

    public Action<TurnstileStates, TurnstileEvents> customerPassAndLock() {
        return context -> System.out.println("当游客通过，锁定旋转门" );
    }

}
```

启动类及测试用例

```
@SpringBootApplication
public class StatemachineApplication implements CommandLineRunner {

    @Autowired
    private StateMachine<TurnstileStates, TurnstileEvents> stateMachine;

    public static void main(String[] args) {
        SpringApplication.run(StatemachineApplication.class, args);
    }

    @Override
    public void run(String... strings) throws Exception {
        stateMachine.start();
        System.out.println("--- coin ---");
        stateMachine.sendEvent(TurnstileEvents.COIN);
        System.out.println("--- coin ---");
        stateMachine.sendEvent(TurnstileEvents.COIN);
        System.out.println("--- push ---");
        stateMachine.sendEvent(TurnstileEvents.PUSH);
        System.out.println("--- push ---");
        stateMachine.sendEvent(TurnstileEvents.PUSH);
        stateMachine.stop();
    }
}
```

结果输出，与上午所描述的状态机所描述的内容一致。

```
--- push ---
解锁旋转门，以便游客能够通过
--- push ---
--- coin ---
当游客通过，锁定旋转门
--- coin ---
```

## 3.2 实用功能

### 3.2.1 状态存储

状态机持久化，实际环境中，当前状态往往都是从持久化介质中实时获取的，Spring Statemachine通过实现StateMachinePersist接口，write和read当前状态机的状态

本例中，使用的是HashMap作为模拟存储介质，正式项目中需要使用真实的状态获取途径

```
@Component
public class BizStateMachinePersist implements StateMachinePersist<TurnstileStates, TurnstileEvents, Integer> {

    static Map<Integer, TurnstileStates> cache = new HashMap<>(16);

    @Override
    public void write(StateMachineContext<TurnstileStates, TurnstileEvents> stateMachineContext, Integer integer) throws Exception {
        cache.put(integer, stateMachineContext.getState());
    }

    @Override
    public StateMachineContext<TurnstileStates, TurnstileEvents> read(Integer integer) throws Exception {
        // 注意状态机的初识状态与配置中定义的一致
        return cache.containsKey(integer) ?
                new DefaultStateMachineContext<>(cache.get(integer), null, null, null, null, "turnstileStateMachine") :
                new DefaultStateMachineContext<>(TurnstileStates.Locked, null, null, null, null, "turnstileStateMachine");
    }
}
```

在StatemachineConfigurer中发布

```
@Autowired
private BizStateMachinePersist bizStateMachinePersist;

@Bean
public StateMachinePersister<TurnstileStates, TurnstileEvents, Integer> stateMachinePersist() {
    return new DefaultStateMachinePersister<>(bizStateMachinePersist);
}
```

在StatemachineApplication中使用，自动注入StateMachinePersister对象，测试用例如下

```
stateMachine.start();

stateMachinePersist.restore(stateMachine, 1);
System.out.println("--- push ---");
stateMachine.sendEvent(TurnstileEvents.PUSH);
stateMachinePersist.persist(stateMachine, 1);

stateMachinePersist.restore(stateMachine, 1);
System.out.println("--- push ---");
stateMachine.sendEvent(TurnstileEvents.PUSH);
stateMachinePersist.persist(stateMachine, 1);

stateMachinePersist.restore(stateMachine, 1);
System.out.println("--- coin ---");
stateMachine.sendEvent(TurnstileEvents.COIN);
stateMachinePersist.persist(stateMachine, 1);

stateMachinePersist.restore(stateMachine, 1);
System.out.println("--- coin ---");
stateMachine.sendEvent(TurnstileEvents.COIN);
stateMachinePersist.persist(stateMachine, 1);

stateMachine.stop();
```

### 3.2.2 动作监听

定义动作监听类，StatemachineMonitor（名称随意），添加注解`@WithStateMachine`。本例中使用id进行状态机绑定，根据文档定义，可以使用name和id两种属性绑定需要监听的状态机实例。如果不定义任何name或者id，默认监听名称为`stateMachine`的状态机。 

```
@WithStateMachine(id = "turnstileStateMachine")
public class StatemachineMonitor {

    @OnTransition
    public void anyTransition() {
        System.out.println("--- OnTransition --- init");
    }

    @OnTransition(target = "Unlocked")
    public void toState1() {
        System.out.println("--- OnTransition --- toState1");
    }

    @OnStateChanged(source = "Unlocked")
    public void fromState1() {
        System.out.println("--- OnTransition --- fromState1");
    }
}
```

其他Context的事件监听，后续文章进行描述，[官网链接](https://docs.spring.io/spring-statemachine/docs/2.0.2.RELEASE/reference/htmlsingle/#sm-context)

### 3.2.2 状态机工厂

实际业务环境中，往往是多线程处理不同的业务ID对应的状态，状态机中利用事件的context传递数据，会出现多线程问题，需要利用状态机工程，利用UUID创建不同状态机。

在StatemachineConfigurer类中，修改`@EnableStateMachine`为`@EnableStateMachineFactory`，同时添加状态机处理动作封装方法，读者可以根据业务场景定制，本例为一种可行方案

```
@Service
public class StatemachineService {

    @Autowired
    private StateMachinePersister<TurnstileStates, TurnstileEvents, Integer> stateMachinePersist;
    @Autowired
    private StateMachineFactory<TurnstileStates, TurnstileEvents> stateMachineFactory;

    public void execute(Integer businessId, TurnstileEvents event, Map<String, Object> context) {
        // 利用随记ID创建状态机，创建时没有与具体定义状态机绑定
        StateMachine<TurnstileStates, TurnstileEvents> stateMachine = stateMachineFactory.getStateMachine(UUID.randomUUID());
        stateMachine.start();
        try {
            // 在BizStateMachinePersist的restore过程中，绑定turnstileStateMachine状态机相关事件监听
            stateMachinePersist.restore(stateMachine, businessId);
            // 本处写法较为繁琐，实际为注入Map<String, Object> context内容到message中
            MessageBuilder<TurnstileEvents> messageBuilder = MessageBuilder
                    .withPayload(event)
                    .setHeader("BusinessId", businessId);
            if (context != null) {
                context.entrySet().forEach(p -> messageBuilder.setHeader(p.getKey(), p.getValue()));
            }
            Message<TurnstileEvents> message = messageBuilder.build();

            // 发送事件，返回是否执行成功
            boolean success = stateMachine.sendEvent(message);
            if (success) {
                stateMachinePersist.persist(stateMachine, businessId);
            } else {
                System.out.println("状态机处理未执行成功，请处理，ID：" + businessId + "，当前context：" + context);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            stateMachine.stop();
        }
    }
}
```

完整示例参见：[链接](https://github.com/shoukai/tools-journey/tree/master/spring-statemachine-sample)

# 4 术语表

* **State Machine**：Main entity driving a collection of states together with regions, transitions and events.
* **State**:A state models a situation during which some invariant condition holds. State is the main entity of a state machine where state changes are driven by an events.（状态模拟一些不变条件成立的情况。State是状态机的主要实体，其中状态更改由事件驱动。）
* **Extended State**:An extended state is a special set of variables kept in a state machine to reduce number of needed states.（扩展状态是状态机中保存的一组特殊变量，用于减少所需状态的数量。）
* **Transition**:A transition is a relationship between a source state and a target state. It may be part of a compound transition, which takes the state machine from one state configuration to another, representing the complete response of the state machine to an occurrence of an event of a particular type.（transition是源状态和目标状态之间的关系。它可能是复合转换的一部分，它将状态机从一个状态配置转移到另一个状态配置，表示状态机对特定类型事件发生的完整响应。）
* **Event**:An entity which is send to a state machine which then drives a various state changes.(发送到状态机的实体，然后驱动各种状态更改。)
* **Initial State**:A special state in which the state machine starts. Initial state is always bound to a particular state machine or a region. A state machine with a multiple regions may have a multiple initial states.（状态机启动的特殊状态。初始状态始终绑定到特定的状态机或区域。具有多个区域的状态机可以具有多个初始状态。）
* **End State**:Also called as a final state is a special kind of state signifying that the enclosing region is completed. If the enclosing region is directly contained in a state machine and all other regions in the state machine also are completed, then it means that the entire state machine is completed.（也被称为最终状态是一种特殊的状态，表示封闭区域已完成。如果封闭区域直接包含在状态机中并且状态机中的所有其他区域也完成，则意味着整个状态机完成。）
* **History State**:A pseudo state which allows a state machine to remember its last active state. Two types of history state exists, shallow which only remember top level state and deep which remembers active states in a sub-machines.（一种伪状态，允许状态机记住其最后一个活动状态。存在两种类型的历史状态，浅层仅记住顶级状态，深度其记住子机器中的活动状态。）
* **Choice State**:A pseudo state which allows to make a transition choice based of i.e. event headers or extended state variables.（一种伪状态，它允许根据事件标题或扩展状态变量进行转换选择。）
* **Junction State**:A pseudo state which is relatively similar to choice state but allows multiple incoming transitions while choice only allows one incoming transition.（一种伪状态，它与选择状态相对类似但允许多个输入转换，而选择仅允许一个输入转换。）
* **Fork State**:A pseudo state which gives a controlled entry into a regions.（一种伪状态，可以控制进入某个区域。）
* **Join State**:A pseudo state which gives a controlled exit from a regions.（
一种伪状态，它可以控制区域的出口。）
* **Entry Point**:A pseudo state which allows a controlled entry into a submachine.（允许受控进入子机的伪状态。）
* **Exit Point**:A pseudo state which allows a controlled exit from a submachine.（允许受控退出子机的伪状态。）
* **Region**:A region is an orthogonal part of either a composite state or a state machine. It contains states and transitions.（区域是复合状态或状态机的正交部分。它包含状态和转换。）
* **Guard**:Is a boolean expression evaluated dynamically based on the value of extended state variables and event parameters. Guard conditions affect the behavior of a state machine by enabling actions or transitions only when they evaluate to TRUE and disabling them when they evaluate to FALSE.（是基于扩展状态变量和事件参数的值动态评估的布尔表达式。保护条件通过仅在评估为TRUE时启用操作或转换并在评估为FALSE时禁用它们来影响状态机的行为。）
* **Action**:A action is a behaviour executed during the triggering of the transition.（动作是在触发转换期间执行的行为。）

# 5 参考示例

* [Spring Statemachine Github](https://github.com/spring-projects)
* [Finite-state machine维基百科](https://en.wikipedia.org/wiki/Finite-state_machine)
* [有限状态机百度百科](https://baike.baidu.com/item/有限状态机/2081914)
* [Spring Statemachine Reference](https://docs.spring.io/spring-statemachine/docs/2.0.2.RELEASE/reference/htmlsingle)





