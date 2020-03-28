---
layout: post
title:  "Feign 05 调用过程"
subtitle: "Feign 源码解析系列"
date:   2019-05-12 00:59:00
author: "Shoukai Huang"
header-img: 'skblog.duiduiche.com/cca353d706148a4631bf373bb3cb32c9.jpg'
header-mask: 0.4
tags: Feign 开源
---

## 简介

本文在《拍拍贷基础框架团队博客-feign源码解析》的基础上，进行了版本更新、修改和补充

Feign makes writing java http clients easier

Feign 是一款 java 的 Restful 客户端组件，Feign使得 Java HTTP 客户端编写更方便。Feign 灵感来源于Retrofit, JAXRS-2.0和 WebSocket。Feign 最初是为了降低统一绑定Denominator 到 HTTP API 的复杂度，不区分是否支持 ReSTfulness。

feign 的基本原理是在接口方法上加注解，定义 rest 请求，构造出接口的动态代理对象，然后通过调用接口方法就可以发送 http 请求，并且自动解析 http 响应为方法返回值，极大的简化了客户端调用 rest api 的代码。

## 接口整理

### 核心类及接口梳理

#### Feign（构建者模式启动类）

初始化入口类，构造器模式初始化参数，通过build参数，初始化 MethodHandler 工厂类和 ParseHandlersByName 类，并创建 ReflectiveFeign 对象。

回顾下 Feign 的 Demo 初始化代码

```java
return Feign.builder()
    .decoder(decoder)
    .errorDecoder(new GitHubErrorDecoder(decoder))
    .logger(new Logger.ErrorLogger())
    .logLevel(Logger.Level.BASIC)
    .target(GitHub.class, "https://api.github.com");
```

其中 target 函数

```java
public <T> T target(Target<T> target) {
  return build().newInstance(target);
}
```

调用 build 函数

```java
public Feign build() {
  // Feign 代理方法初始化所需参数
  SynchronousMethodHandler.Factory synchronousMethodHandlerFactory =
      new SynchronousMethodHandler.Factory(client, retryer, requestInterceptors, logger,
          logLevel, decode404, closeAfterDecode, propagationPolicy);
  // Feign 代理类初始化所需参数
  ParseHandlersByName handlersByName =
      new ParseHandlersByName(contract, options, encoder, decoder, queryMapEncoder,
          errorDecoder, synchronousMethodHandlerFactory);
  // 初始化 ReflectiveFeign 对象
  return new ReflectiveFeign(handlersByName, invocationHandlerFactory, queryMapEncoder);
}
```

#### ReflectiveFeign（核心处理类）

获得 Feign 传入的全部参数，解析方法注解及参数、解析接口注解及参数，创建 Java 动态代理类

```java
@Override
public <T> T newInstance(Target<T> target) {
  // targetToHandlersByName 是构造器传入的 ParseHandlersByName 对象，根据target 对象生成 MethodHandler 映射。里面包含：Contract、Options、Encoder、Decoder、ErrorDecoder、QueryMapEncoder及SynchronousMethodHandler.Factory
  Map<String, MethodHandler> nameToHandler = targetToHandlersByName.apply(target);
  Map<Method, MethodHandler> methodToHandler = new LinkedHashMap<Method, MethodHandler>();
  List<DefaultMethodHandler> defaultMethodHandlers = new LinkedList<DefaultMethodHandler>();
  // 遍历接口所有方法，构建Method->MethodHandler的映射
  for (Method method : target.type().getMethods()) {
    if (method.getDeclaringClass() == Object.class) {
      continue;
    } else if (Util.isDefault(method)) {
      // 接口default方法的Handler，这类方法直接调用
      DefaultMethodHandler handler = new DefaultMethodHandler(method);
      defaultMethodHandlers.add(handler);
      methodToHandler.put(method, handler);
    } else {
      methodToHandler.put(method, nameToHandler.get(Feign.configKey(target.type(), method)));
    }
  }
  // 这里 factory 是构造其中传入的，创建 InvocationHandler
  InvocationHandler handler = factory.create(target, methodToHandler);
  // java的动态代理
  T proxy = (T) Proxy.newProxyInstance(target.type().getClassLoader(),
      new Class<?>[] {target.type()}, handler);
  // 将default方法直接绑定到动态代理上
  for (DefaultMethodHandler defaultMethodHandler : defaultMethodHandlers) {
    defaultMethodHandler.bindTo(proxy);
  }
  return proxy;
}
```

#### InvocationHandlerFactory

控制反射方法调度

>Controls reflective method dispatch.

包含 MethodHandler 接口定义及一个当前接口的默认实现类 

#### InvocationHandlerFactory.Default


```java
static final class Default implements InvocationHandlerFactory {
  @Override
  public InvocationHandler create(Target target, Map<Method, MethodHandler> dispatch) {
    return new ReflectiveFeign.FeignInvocationHandler(target, dispatch);
  }
}
```

#### ReflectiveFeign.ParseHandlersByName

存储 Contract、Options、Encoder、Decoder、ErrorDecoder、QueryMapEncoder、SynchronousMethodHandler.Factory等初始化内容。并提供 apply() 方法解析得到 MethodHandler

#### ReflectiveFeign.FeignInvocationHandler

包含：Target target 和 Map<Method, MethodHandler> dispatch

实现 Java 动态代理接口 InvocationHandler。

#### Target.HardCodedTarget

默认实现的请求转换器，接口对象的实例化对象

#### InvocationHandlerFactory.MethodHandler

Like InvocationHandler.invoke(Object, java.lang.reflect.Method, Object[]), except for a single method.

java7在JSR 292中增加了对动态类型语言的支持，使java也可以像C语言那样将方法作为参数传递，其实现在lava.lang.invoke包中。MethodHandle作用类似于反射中的Method类，但它比Method类要更加灵活和轻量级。

### 扩展接口梳理

#### Contract（契约）

> Defines what annotations and values are valid on interfaces.

Contract的作用是解析接口方法，生成 Rest 定义。feign 默认使用自己的定义的注解，还提供了

* JAXRSContract javax.ws.rs注解接口实现
* SpringContract是spring cloud提供SpringMVC注解实现方式。

#### InvocationHandler（动态代理 handler）

通过 InvocationHandlerFactory 注入到 Feign.Builder 中，feign提供了 Hystrix 的扩展，实现 Hystrix 接入

#### Encoder（请求body编码器）

> Encodes an object into an HTTP request body. Like javax.websocket.Encoder. Encoder is used when a method parameter has no @Param annotation. 

For example: 

```java
@POST
@Path("/")
void create(User user);
```

Example implementation: 

```java
public class GsonEncoder implements Encoder {
  private final Gson gson;

  public GsonEncoder(Gson gson) {
    this.gson = gson;
  }

  @Override
  public void encode(Object object, Type bodyType, RequestTemplate template) {
    template.body(gson.toJson(object, bodyType));
  }
}
```

feign已经提供扩展包含：

* 默认编码器，只能处理String和byte[]
* json编码器GsonEncoder、JacksonEncoder
* XML编码器JAXBEncoder

#### Decoder（http响应解码器）

> Decodes an HTTP response into a single object of the given type. Invoked when Response.status() is in the 2xx range and the return type is neither void nor Response.

Example Implementation:

```java
public class GsonDecoder implements Decoder {
  private final Gson gson = new Gson();

  @Override
  public Object decode(Response response, Type type) throws IOException {
    try {
      return gson.fromJson(response.body().asReader(), type);
    } catch (JsonIOException e) {
      if (e.getCause() != null &&
          e.getCause() instanceof IOException) {
        throw IOException.class.cast(e.getCause());
      }
      throw e;
    }
  }
}
```

最基本的有：

* json解码器 GsonDecoder、JacksonDecoder
* XML解码器 JAXBDecoder
* Stream流解码器 StreamDecoder

#### Target（请求转换器）

> Similar to javax.ws.rs.client.WebTarget, as it produces requests. However, RequestTemplate is a closer match to WebTarget.

WebTarget

> A resource target identified by the resource URI.

feign提供的实现有：

* HardCodedTarget：默认Target，不做任何处理。
* LoadBalancingTarget：使用Ribbon进行客户端路由

#### Client（发送http请求的客户端）

> Submits HTTP requests. Implementations are expected to be thread-safe.

feign 提供的 Client 实现有：

* Client.Default：默认实现，使用 java api 的 HttpClientConnection：发送 http 请求
* ApacheHttpClient：使用apache的Http客户端发送请求
* OkHttpClient：使用OKHttp客户端发送请求
* RibbonClient：使用Ribbon进行客户端路由

#### RequestInterceptor（请求拦截器）

可以配置零个或多个RequestInterceptors，以便为所有请求添加标头。关于拦截器的使用顺序，我们不予保证。一旦应用了拦截器，就会调用Target.apply（RequestTemplate）来创建通过Client.execute（Request，feign.Request.Options）发送的不可变http请求。

>Zero or more RequestInterceptors may be configured for purposes such as adding headers to all requests. No guarantees are give with regards to the order that interceptors are applied. Once interceptors are applied, Target.apply(RequestTemplate) is called to create the immutable http request sent via Client.execute(Request, feign.Request.Options). 

调用客户端发请求前，修改RequestTemplate，比如为所有请求添加Header就可以用拦截器实现。

示例：

```java
public void apply(RequestTemplate input) {
  input.header("X-Auth", currentToken);
}
```

#### Retryer（重试策略）

clone 每次调用 Client.execute（Request，feign.Request.Options）。实现可以保持状态以确定重试操作是否应该继续。

> Cloned for each invocation to Client.execute(Request, feign.Request.Options). Implementations may keep state to determine if retry operations should continue or not.

默认的策略是 Retryer.Default，包含3个参数：间隔、最大间隔和重试次数，第一次失败重试前会 sleep 输入的间隔时间的，后面每次重试 sleep 时间是前一次的 1.5 倍，超过最大时间或者最大重试次数就失败。

## 参考

* [feign源码解析](http://techblog.ppdai.com/2018/05/14/20180514/)








