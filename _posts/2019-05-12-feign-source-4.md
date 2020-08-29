---
layout: post
title:  "开源软件：Feign 04 调用过程"
subtitle: "Feign 源码解析系列"
date:   2019-05-12 00:59:00
author: "Shoukai Huang"
header-img: 'skblog.duiduiche.com/cca353d706148a4631bf373bb3cb32c9.jpg'
header-mask: 0.4
tags: Feign 开源软件
---


## 简介

本文在《拍拍贷基础框架团队博客-feign源码解析》的基础上，进行了版本更新、修改和补充

Feign makes writing java http clients easier

Feign 是一款 java 的 Restful 客户端组件，Feign使得 Java HTTP 客户端编写更方便。Feign 灵感来源于Retrofit, JAXRS-2.0和 WebSocket。Feign 最初是为了降低统一绑定Denominator 到 HTTP API 的复杂度，不区分是否支持 ReSTfulness。

feign 的基本原理是在接口方法上加注解，定义 rest 请求，构造出接口的动态代理对象，然后通过调用接口方法就可以发送 http 请求，并且自动解析 http 响应为方法返回值，极大的简化了客户端调用 rest api 的代码。

## 基本实现

### 接口调用

为方便理解，分析完feign源码后，我将feign执行过程分成三层，如下图：

![](http://skblog.duiduiche.com/794d7d335d200c031765afa0a422ecc7.jpg)

三层分别为：

* 代理层：动态代理调用层
* 转换层：方法转http请求，解码http响应
* 网络层：http请求发送

java 动态代理接口方法调用，会调用到 InvocaHandler 的 invoke 方法，feign 里面实现类是 FeignInvocationHandler，invoke 代码如下：

```java
static class FeignInvocationHandler implements InvocationHandler {
   private final Target target;
   private final Map<Method, MethodHandler> dispatch;

   FeignInvocationHandler(Target target, Map<Method, MethodHandler> dispatch) {
     this.target = checkNotNull(target, "target");
     this.dispatch = checkNotNull(dispatch, "dispatch for %s", target);
   }

   @Override
   public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
     if ("equals".equals(method.getName())) {
       try {
         Object otherHandler =
             args.length > 0 && args[0] != null ? Proxy.getInvocationHandler(args[0]) : null;
         return equals(otherHandler);
       } catch (IllegalArgumentException e) {
         return false;
       }
     } else if ("hashCode".equals(method.getName())) {
       return hashCode();
     } else if ("toString".equals(method.getName())) {
       return toString();
     }
     return dispatch.get(method).invoke(args);
   }
}
```

根据方法找到 MethodHandler，除接口的 default 方法外，找到的是 SynchronousMethodHandler 对象，然后调用SynchronousMethodHandlerd.invoke 方法：

```java
final class SynchronousMethodHandler implements MethodHandler {
  //...
  @Override
  public Object invoke(Object[] argv) throws Throwable {
    //buildTemplateFromArgs 是 RequestTemplate 工程对象，根据方法参数创建 RequestTemplate
    RequestTemplate template = buildTemplateFromArgs.create(argv);
    Retryer retryer = this.retryer.clone();
    while (true) {
      try {
        return executeAndDecode(template);
      } catch (RetryableException e) {
        try {
          // 执行和解码
          retryer.continueOrPropagate(e);
        } catch (RetryableException th) {
          Throwable cause = th.getCause();
          if (propagationPolicy == UNWRAP && cause != null) {
            throw cause;
          } else {
            throw th;
          }
        }
        continue;
      }
    }
  }

  Object executeAndDecode(RequestTemplate template) throws Throwable {
    // RequestTemplate转换为Request
    Request request = targetRequest(template);

    Response response;
    long start = System.nanoTime();
    try {
      response = client.execute(request, options);
    } catch (IOException e) {
      throw errorExecuting(request, e);
    }
    long elapsedTime = TimeUnit.NANOSECONDS.toMillis(System.nanoTime() - start);

    boolean shouldClose = true;
    try {
      if (Response.class == metadata.returnType()) {
        // 如果接口方法返回的是Response类
        if (response.body() == null) {
          // body为空，直接返回
          return response;
        }
        if (response.body().length() == null ||
            response.body().length() > MAX_RESPONSE_BUFFER_SIZE) {
          // body不为空，且length>最大缓存值，返回response，但是不能关闭response
          shouldClose = false;
          return response;
        }
        // Ensure the response body is disconnected 读取body字节数组，返回response
        byte[] bodyData = Util.toByteArray(response.body().asInputStream());
        return response.toBuilder().body(bodyData).build();
      }
      if (response.status() >= 200 && response.status() < 300) {
        // 响应成功
        if (void.class == metadata.returnType()) {
          return null;
        } else {
          // 解码response，直接调用decoder解码
          Object result = decode(response);
          shouldClose = closeAfterDecode;
          return result;
        }
      } else if (decode404 && response.status() == 404 && void.class != metadata.returnType()) {
        // 404解析
        Object result = decode(response);
        shouldClose = closeAfterDecode;
        return result;
      } else {
        // 其他返回码，使用errorDecoder解析，抛出异常
        throw errorDecoder.decode(metadata.configKey(), response);
      }
    } catch (IOException e) {
      if (logLevel != Logger.Level.NONE) {
        logger.logIOException(metadata.configKey(), logLevel, e, elapsedTime);
      }
      throw errorReading(request, response, e);
    } finally {
      if (shouldClose) {
        // 是否需要关闭response，根据Feign.Builder 参数设置是否要关闭流
        ensureClosed(response.body());
      }
    }
}
```

为了更直观的了解 RequestTemplate template = buildTemplateFromArgs.create(argv) 的内容，对示例代码进行断点。

调用代码

```java
github.contributors("openfeign", "some-unknown-project");
```

入参内容：openfeign 和 some-unknown-project

![](http://skblog.duiduiche.com/fc5eac5c6affc21c6d6c9c9484efde29.jpg)

获得 RequestTemplate 对象内容

![](http://skblog.duiduiche.com/214fb6b25bd779042e7eff00d9c0c99b.jpg)

过程比较简单，生成 RquestTemplate -> 转换为Request -> client发请求 -> Decoder解析Response

#### RequestTemplate 构建过程

先看看RequestTemplate的结构：

```java
public final class RequestTemplate implements Serializable {

  private static final Pattern QUERY_STRING_PATTERN = Pattern.compile("(?<!\\{)\\?");
  // 请求参数 ?后面的name=value
  private final Map<String, QueryTemplate> queries = new LinkedHashMap<>();
  // 请求 Header
  private final Map<String, HeaderTemplate> headers = new TreeMap<>(String.CASE_INSENSITIVE_ORDER);
  private String target;
  private boolean resolved = false;
  private UriTemplate uriTemplate;
  // 请求方法 GET/POST等
  private HttpMethod method;
  private transient Charset charset = Util.UTF_8;
  private Request.Body body = Request.Body.empty();
  private boolean decodeSlash = true;
  private CollectionFormat collectionFormat = CollectionFormat.EXPLODED;

  // ...
}
```

在 SynchronousMethodHandler.invoke 方法中生成 RequestTemplate

```java
//buildTemplateFromArgs是RequestTemplate.Factory实现类
RequestTemplate template = buildTemplateFromArgs.create(argv);
```

RequestTemplate.Factory 有三个实现类：

* BuildTemplateByResolvingArgs：RequestTemplate工厂
* BuildEncodedTemplateFromArgs：BuildTemplateByResolvingArgs的子类 重载resolve方法，解析form表单请求
* BuildFormEncodedTemplateFromArgs：BuildTemplateByResolvingArgs的子类，重载resolve方法，解析body请求

BuildTemplateByResolvingArgs 创建 RequestTemplate 的 create 方法：

```java
// BuildTemplateByResolvingArgs 实现 RequestTemplate.Factory 的方法
@Override
public RequestTemplate create(Object[] argv) {
  RequestTemplate mutable = RequestTemplate.from(metadata.template());
  if (metadata.urlIndex() != null) {
    // 插入接口方法参数中的URI
    int urlIndex = metadata.urlIndex();
    checkArgument(argv[urlIndex] != null, "URI parameter %s was null", urlIndex);
    mutable.target(String.valueOf(argv[urlIndex]));
  }
  Map<String, Object> varBuilder = new LinkedHashMap<String, Object>();
  // 方法参数位置和请求定义的参数名称的map
  for (Entry<Integer, Collection<String>> entry : metadata.indexToName().entrySet()) {
    // 将方法参数值和定义的请求参数进行映射，varBuilder
    int i = entry.getKey();
    Object value = argv[entry.getKey()];
    if (value != null) { // Null values are skipped.
      if (indexToExpander.containsKey(i)) {
        value = expandElements(indexToExpander.get(i), value);
      }
      for (String name : entry.getValue()) {
        varBuilder.put(name, value);
      }
    }
  }
  // 解析RequestTemplate
  RequestTemplate template = resolve(argv, mutable, varBuilder);
  // 解析queryMap，这块代码有些奇怪，为什么单独把queryMap放在这里解析，而不是在resolve方法中，或者在RequestTemplate中
  if (metadata.queryMapIndex() != null) {
    // add query map parameters after initial resolve so that they take
    // precedence over any predefined values
    Object value = argv[metadata.queryMapIndex()];
    Map<String, Object> queryMap = toQueryMap(value);
    template = addQueryMapQueryParameters(queryMap, template);
  }
  // 解析headerMap定义的参数
  if (metadata.headerMapIndex() != null) {
    template =
        addHeaderMapHeaders((Map<String, Object>) argv[metadata.headerMapIndex()], template);
  }

  return template;
}
```

#### RquestTemplate转换Request

先来看看Request的结构，完整的http请求信息的定义：

```java
public final class Request {
  private final HttpMethod httpMethod;
  private final String url;
  private final Map<String, Collection<String>> headers;
  private final Body body;
  // ...
}
```

SynchronousMethodHandler 的 targetRequest 方法将 RequestTemplate 转换为 Request

```java
Request targetRequest(RequestTemplate template) {
  // 先应用所用拦截器，拦截器是在Feign.Builder中传入的，拦截器可以修改RequestTemplate信息
  for (RequestInterceptor interceptor : requestInterceptors) {
    interceptor.apply(template);
  }
  // 调用Target的apply方法，默认Target是HardCodedTarget
  return target.apply(template);
}
```

这块先应用所有拦截器，然后 target 的 apply 方法。拦截器和 target 都是扩展点，拦截器可以在构造好 RequestTemplate 后和发请求前修改请求信息，target 默认使用 HardCodedTarget 直接发请求，feign还提供了 LoadBalancingTarget，适配Ribbon 来发请求，实现客户端的负载均衡。

**HardCodedTarget**

```java
/* no authentication or other special activity. just insert the url. */
@Override
// HardCodedTarget的apply方法
public Request apply(RequestTemplate input) {
  if (input.url().indexOf("http") != 0) {
    input.target(url());
  }
  return input.request();
}
```

**RequestTemplate**: Request Builder for an HTTP Target.

```java
public Request request() {
  if (!this.resolved) {
    throw new IllegalStateException("template has not been resolved.");
  }
  return Request.create(this.method, this.url(), this.headers(), this.requestBody());
}
```

**Request**: An immutable request to an http server.

```java
public static Request create(HttpMethod httpMethod,
                             String url,
                             Map<String, Collection<String>> headers,
                             Body body) {
  return new Request(httpMethod, url, headers, body);
}
```

从代码上可以看到，RequestTemplate基本上直接转为Request，没有做什么逻辑操作。对比下 LoadBalancingTarget

```java
public Request apply(RequestTemplate input) {
  //选取一个Server，lb是Ribbon的AbstractLoadBalancer类
  Server currentServer = lb.chooseServer(null);
  //生成url
  String url = format("%s://%s%s", scheme, currentServer.getHostPort(), path);
  input.insert(0, url);
  try {
    //生成Request
    return input.request();
  } finally {
    lb.getLoadBalancerStats().incrementNumRequests(currentServer);
  }
}
```

可以看到，非常简单的几行代码，只要修改请求的url就能实现客户端负载均衡。

#### http请求发送

SynchronousMethodHandler中构造好Request后，直接调用client的execute方法发送请求：

```java
response = client.execute(request, options);
```

client是一个Client接口，默认实现类是Client.Default，使用java api中的HttpURLConnection发送http请求。feign还实现了：

* ApacheHttpClient
* OkHttpClient
* RibbonClient

使用RibbonClient跟使用LoadBalancingTarget作用都是实现客户端负载均衡，RibbonClient实现稍微复杂些。

### 接口调用过程总结

我们再将接口调用过程捋一遍：

1. 接口的动态代理Proxy调用接口方法会执行的FeignInvocationHandler
2. FeignInvocationHandler通过方法签名在属性Map<Method, MethodHandler> dispatch中找到SynchronousMethodHandler，调用invoke方法
3. SynchronousMethodHandler的invoke方法根据传入的方法参数，通过自身属性工厂对象RequestTemplate.Factory创建RequestTemplate，工厂里面会用根据需要进行Encode
4. SynchronousMethodHandler遍历自身属性RequestInterceptor列表，对RequestTemplate进行改造
5. SynchronousMethodHandler调用自身Target属性的apply方法，将RequestTemplate转换为Request对象
6. SynchronousMethodHandler调用自身Client的execute方法，传入Request对象
7. Client将Request转换为http请求，发送后将http响应转换为Response对象
8. SynchronousMethodHandler调用Decoder的方法对Response对象解码后返回
9. 返回的对象最后返回到Proxy

时序图如下：

![](http://skblog.duiduiche.com/33a055b351ac80b0d86614de7fb3c429.jpg)


## 参考

* [feign源码解析](http://techblog.ppdai.com/2018/05/14/20180514/)