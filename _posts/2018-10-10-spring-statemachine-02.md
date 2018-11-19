---
layout: post
title:  "Spring Statemachine Part Two"
subtitle: "更多功能"
date:   2018-09-29 8:00:00
background: 'http:/\/skblog.duiduiche.com/02.jpg'
---
# Spring Statemachine 更多功能

## 1 功能介绍

### 1.1 Hierarchical States

通过 withStates() 和 parent() 定义层次状态

```
@Configuration
@EnableStateMachine
public class Config2 extends EnumStateMachineConfigurerAdapter<States, Events> {

    @Override
    public void configure(StateMachineStateConfigurer<States, Events> states)
            throws Exception {
        states
            .withStates()
                .initial(States.S1)
                .state(States.S1)
                .and()
                .withStates()
                    .parent(States.S1)
                    .initial(States.S2)
                    .state(States.S2);
    }

}
```

为了进一步分析层次状态的功能特性，引用了[参考手册Showcase的例子](https://docs.spring.io/spring-statemachine/docs/2.0.2.RELEASE/reference/htmlsingle/#statemachine-examples-showcase)，状态图定义如下
![statechart2](http://skblog.duiduiche.com/statechart2.png){:height="100%" width="100%" }

运行官网示例，得到结果如下

![1539185678943](http://skblog.duiduiche.com/1539185678943.jpg){:height="100%" width="100%" }

根据测试结果得到初步的理解如下：

* 初识状态在每次状态变更后生效，进入S0后会自动进入S1，进而进入S11；
* 当处于子层次（如：S11），事件C发生，因为S11属于S1，满足C条件；
* 代码测试：当前处于子层次（如：S211），事件G发生，会进入S0，然后根据初识状态进入S2（不是S1，与状态机启动时不一致），状态机有[历史状态](https://docs.spring.io/spring-statemachine/docs/2.0.2.RELEASE/reference/htmlsingle/#history-state)；
* 利用例子测试会出现，一次事件连续两次进入S1的情况（怀疑为Bug），继续跟踪；
* 复杂状态机，需要进行全方位测试，确保状态机中未了解特性影响预期结果。

### 1.2 Using Actions

Action 是与状态机交互和协作的最有用的组件之一。动作可以在状态机中的各个位置执行，并且可以在状态生命周期中执行，例如进入或退出状态或转换期间。基本配置示例：

```
@Override
public void configure(StateMachineStateConfigurer<States, Events> states)
        throws Exception {
    states
        .withStates()
            .initial(States.SI)
            .state(States.S1, action1(), action2())
            .state(States.S2, action1(), action2())
            .state(States.S3, action1(), action3());
}
```

* action1()在进入States.S1、States.S2和States.S3时生效；
* action2()在离开States.S1和States.S2时生效；
* action3()在离开States.S3时生效；

Action的三种使用

**用法1：anonymous function**

```
@Bean
public Action<States, Events> action1() {
    return new Action<States, Events>() {

        @Override
        public void execute(StateContext<States, Events> context) {
        }
    };
}
```

**用法2：own implementation**

```
@Bean
public BaseAction action2() {
    return new BaseAction();
}

public class BaseAction implements Action<States, Events> {

    @Override
    public void execute(StateContext<States, Events> context) {
    }
}

```

**用法3：SpEL expression**

```
@Bean
public SpelAction action3() {
    ExpressionParser parser = new SpelExpressionParser();
    return new SpelAction(
            parser.parseExpression(
                    "stateMachine.sendEvent(T(org.springframework.statemachine.docs.Events).E1)"));
}

public class SpelAction extends SpelExpressionAction<States, Events> {

    public SpelAction(Expression expression) {
        super(expression);
    }
}
```
能够发送事件：Events.E1在action中


### 1.3 Using Guards

Guard是事件的拦截器，根据Guard的判断结果，决定是否执行后续操作，基本配置如下：

```
@Override
public void configure(StateMachineTransitionConfigurer<States, Events> transitions)
        throws Exception {
    transitions
        .withExternal()
            .source(States.SI).target(States.S1)
            .event(Events.E1)
            .guard(guard1())
            .and()
        .withExternal()
            .source(States.S1).target(States.S2)
            .event(Events.E1)
            .guard(guard2())
            .and()
        .withExternal()
            .source(States.S2).target(States.S3)
            .event(Events.E2)
            .guardExpression("extendedState.variables.get('myvar')");
}
```

同Action，也是三种基本用法

**用法1：anonymous function**

```
@Bean
public Guard<States, Events> guard1() {
    return new Guard<States, Events>() {

        @Override
        public boolean evaluate(StateContext<States, Events> context) {
            return true;
        }
    };
}
```

**用法2：own implementation**

```
@Bean
public BaseGuard guard2() {
    return new BaseGuard();
}

public class BaseGuard implements Guard<States, Events> {

    @Override
    public boolean evaluate(StateContext<States, Events> context) {
        return false;
    }
}
```

**用法2：Expression**

通过表达式，判断是否为TRUE

```
guardExpression("extendedState.variables.get('myvar')");
```


# 2 参考示例
* [Spring Statemachine Reference](https://docs.spring.io/spring-statemachine/docs/2.0.2.RELEASE/reference/htmlsingle)







