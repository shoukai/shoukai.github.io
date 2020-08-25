---
layout: post
title:  "开源软件：Feign 02 手册翻译"
subtitle: "Feign 源码解析系列"
date:   2019-05-10 00:59:00
author: "Shoukai Huang"
header-img: 'skblog.duiduiche.com/cca353d706148a4631bf373bb3cb32c9.jpg'
header-mask: 0.4
tags: Feign
---

# 简介

Feign makes writing java http clients easier

Feign 是一款 java 的 Restful 客户端组件，Feign使得 Java HTTP 客户端编写更方便。Feign 灵感来源于Retrofit, JAXRS-2.0和 WebSocket。Feign 最初是为了降低统一绑定Denominator 到 HTTP API 的复杂度，不区分是否支持 ReSTfulness。

>Feign is a Java to HTTP client binder inspired by [Retrofit](https://github.com/square/retrofit), [JAXRS-2.0](https://jax-rs-spec.java.net/nonav/2.0/apidocs/index.html), and [WebSocket](http://www.oracle.com/technetwork/articles/java/jsr356-1937161.html).  Feign's first goal was reducing the complexity of binding [Denominator](https://github.com/Netflix/Denominator) uniformly to HTTP APIs regardless of [ReSTfulness](http://www.slideshare.net/adrianfcole/99problems).

### Why Feign and not X?

你可以使用 Jersey 和 CXF 这些来写一个 Rest 或 SOAP 服务的java客服端。你也可以直接使用 Apache HttpClient 来实现。但是 Feign 的目的是尽量的减少资源和代码来实现和 HTTP API 的连接。通过自定义的编码解码器以及错误处理，你可以编写任何基于文本的 HTTP API。

>Feign uses tools like Jersey and CXF to write java clients for ReST or SOAP services. Furthermore, Feign allows you to write your own code on top of http libraries such as Apache HC. Feign connects your code to http APIs with minimal overhead and code via customizable decoders and error handling, which can be written to any text-based http API.

### Feign工作机制

Feign 通过注解注入一个模板化请求进行工作。只需在发送之前关闭它，参数就可以被直接的运用到模板中。然而这也限制了 Feign，只支持文本形式的API，它在响应请求等方面极大的简化了系统。同时，它也是十分容易进行单元测试的。

>Feign works by processing annotations into a templatized request. Arguments are applied to these templates in a straightforward fashion before output.  Although Feign is limited to supporting text-based APIs, it dramatically simplifies system aspects such as replaying requests. Furthermore, Feign makes it easy to unit test your conversions knowing this.

### Java 版本兼容性

Feign 10.x and above are built on Java 8 and should work on Java 9, 10, and 11.  For those that need JDK 6 compatibility, please use Feign 9.x

### Basics

基本的使用如下所示，一个对于canonical Retrofit sample的适配。

```java
interface GitHub {
  @RequestLine("GET /repos/{owner}/{repo}/contributors")
  List<Contributor> contributors(@Param("owner") String owner, @Param("repo") String repo);
}

public static class Contributor {
  String login;
  int contributions;
}

public class MyApp {
  public static void main(String... args) {
    GitHub github = Feign.builder()
                         .decoder(new GsonDecoder())
                         .target(GitHub.class, "https://api.github.com");
  
    // Fetch and print a list of the contributors to this library.
    List<Contributor> contributors = github.contributors("OpenFeign", "feign");
    for (Contributor contributor : contributors) {
      System.out.println(contributor.login + " (" + contributor.contributions + ")");
    }
  }
}
```

### 接口注释

Feign注释定义了接口与底层客户端应该如何工作之间的“契约”。 Feign的默认合约定义了以下注释：

| Annotation     | Interface Target | Usage |
|----------------|------------------|-------|
| `@RequestLine` | Method           | Defines the `HttpMethod` and `UriTemplate` for request.  `Expressions`, values wrapped in curly-braces `{expression}` are resolved using their corresponding `@Param` annotated parameters. |
| `@Param`       | Parameter        | Defines a template variable, whose value will be used to resolve the corresponding template `Expression`, by name. |
| `@Headers`     | Method, Type     | Defines a `HeaderTemplate`; a variation on a `UriTemplate`.  that uses `@Param` annotated values to resolve the corresponding `Expressions`.  When used on a `Type`, the template will be applied to every request.  When used on a `Method`, the template will apply only to the annotated method. |
| `@QueryMap`    | Parameter        | Defines a `Map` of name-value pairs, or POJO, to expand into a query string. |
| `@HeaderMap`   | Parameter        | Defines a `Map` of name-value pairs, to expand into `Http Headers` |
| `@Body`        | Method           | Defines a `Template`, similar to a `UriTemplate` and `HeaderTemplate`, that uses `@Param` annotated values to resolve the corresponding `Expressions`.|

### 模板和表达式

Feign 表达式表示由 URI 模板 -  RFC 6570定义的简单字符串表达式（级别1）。表达式使用扩展
它们对应的Param注释方法参数。

*Example*

```java
public interface GitHub {
  
  @RequestLine("GET /repos/{owner}/{repo}/contributors")
  List<Contributor> getContributors(@Param("owner") String owner, @Param("repo") String repository);
  
  class Contributor {
    String login;
    int contributions;
  }
}

public class MyApp {
  public static void main(String[] args) {
    GitHub github = Feign.builder()
                         .decoder(new GsonDecoder())
                         .target(GitHub.class, "https://api.github.com");
    
    /* The owner and repository parameters will be used to expand the owner and repo expressions
     * defined in the RequestLine.
     * 
     * the resulting uri will be https://api.github.com/repos/OpenFeign/feign/contributors
     */
    github.contributors("OpenFeign", "feign");
  }
}
```

表达式必须用大括号{}括起来，并且可以包含正则表达式模式，用冒号分隔：限制
已解决的价值示例所有者必须是字母。 {所有者：[A-ZA-Z] *}

#### Request Parameter Expansion

RequestLine和QueryMap模板遵循针对1级模板的URI模板 -  RFC 6570规范，其中指定了以下内容：

* 未解决的表达式被省略。
* 所有文字和变量值都是pct编码的，如果尚未编码或通过@Param注释标记编码。

See [Advanced Usage](#advanced-usage) for more examples.

> **What about slashes? `/`**
>
> @RequestLine和@QueryMap模板默认不对斜杠/字符进行编码。要更改此行为，请将@RequestLine上的decodeSlash属性设置为false。

> **What about plus? `+`**
>
> 根据URI规范，URI的路径和查询段中都允许使用+符号，但是处理查询上的符号可能不一致。在一些遗留系统中，+等同于空格。 Feign采用现代系统的方法，其中a+符号不应表示空格，并且在查询字符串上找到时显式编码为％2B。
> 如果您希望使用+作为空格，则使用文字字符或直接将值编码为％20
 
##### 自定义扩展

@Param注释具有可选的属性扩展器，允许完全控制单个参数的扩展。
expandder属性必须引用实现Expander接口的类：

```java
public interface Expander {
    String expand(Object value);
}
```
该方法的结果遵循上述相同的规则。如果结果为null或空字符串，
该值被省略。如果该值不是pct编码的，那么它将是。有关更多示例，请参阅自定义@Param扩展。

#### Request Headers Expansion 


Headers和HeaderMap模板遵循与请求参数扩展相同的规则,进行以下更改：

* 未解决的表达式被省略。如果结果为空标头值，则删除整个标头。
* 没有执行pct编码。

See [Headers](#headers) for examples.

> 关于@Param参数及其名称的注意事项：
> 所有具有相同名称的表达式，无论它们在@RequestLine，@QueryMap，@BodyTemplate或@Headers上的位置如何，都将解析为相同的值。
> 在以下示例中，contentType的值将用于解析标头和路径表达式：

在设计接口时请记住这一点。

#### Request Body Expansion

正文模板遵循与请求参数扩展相同的规则，进行以下更改：

* 未解决的表达式被省略。
* 扩展值在放入请求主体之前不会通过编码器传递。
* 必须指定Content-Type标头。有关示例，请参见正文模板

---

### 自定义

Feign 有许多可以自定义的方面。举个简单的例子，你可以使用 Feign.builder() 来构造一个拥有你自己组件的API接口。如下:

```java
interface Bank {
  @RequestLine("POST /account/{id}")
  Account getAccountInfo(@Param("id") String id);
}

public class BankService {
  public static void main(String[] args) {
    Bank bank = Feign.builder().decoder(
        new AccountDecoder())
        .target(Bank.class, "https://api.examplebank.com");
  }
}
```

### 多种接口

Feign可以提供多种API接口，这些接口都被定义为 Target<T> (默认的实现是 HardCodedTarget<T>), 它允许在执行请求前动态发现和装饰该请求。

举个例子，下面的这个模式允许使用当前url和身份验证token来装饰每个发往身份验证中心服务的请求。

```java
public class CloudService {
  public static void main(String[] args) {
    CloudDNS cloudDNS = Feign.builder()
      .target(new CloudIdentityTarget<CloudDNS>(user, apiKey));
  }
  
  class CloudIdentityTarget extends Target<CloudDNS> {
    /* implementation of a Target */
  }
}
```

### Examples

Feign 包含了 GitHub 和 Wikipedia 客户端的实现样例.相似的项目也同样在实践中运用了Feign。尤其是它的示例后台程序example daemon。

---

### Integrations
Feign 可以和其他的开源工具集成工作。你可以将这些开源工具集成到 Feign 中来。目前已经有的一些模块如下:

### Gson
Gson 包含了一个编码器和一个解码器，这个可以被用于JSON格式的API。
添加 GsonEncoder 以及 GsonDecoder 到你的 Feign.Builder 中， 如下:

```java
public class Example {
  public static void main(String[] args) {
    GsonCodec codec = new GsonCodec();
    GitHub github = Feign.builder()
                         .encoder(new GsonEncoder())
                         .decoder(new GsonDecoder())
                         .target(GitHub.class, "https://api.github.com");
  }
}
```

### Jackson

Jackson 包含了一个编码器和一个解码器，这个可以被用于JSON格式的API。
添加 JacksonEncoder 以及 JacksonDecoder 到你的 Feign.Builder 中， 如下:

```java
public class Example {
  public static void main(String[] args) {
      GitHub github = Feign.builder()
                     .encoder(new JacksonEncoder())
                     .decoder(new JacksonDecoder())
                     .target(GitHub.class, "https://api.github.com");
  }
}
```

### Sax

SaxDecoder 用于解析XML,并兼容普通JVM和Android。下面是一个配置sax来解析响应的例子:

```java
public class Example {
  public static void main(String[] args) {
      Api api = Feign.builder()
         .decoder(SAXDecoder.builder()
                            .registerContentHandler(UserIdHandler.class)
                            .build())
         .target(Api.class, "https://apihost");
    }
}
```

### JAXB

JAXB 包含了一个编码器和一个解码器，这个可以被用于XML格式的API。
添加 JAXBEncoder 以及 JAXBDecoder 到你的 Feign.Builder 中， 如下:

```java
public class Example {
  public static void main(String[] args) {
    Api api = Feign.builder()
             .encoder(new JAXBEncoder())
             .decoder(new JAXBDecoder())
             .target(Api.class, "https://apihost");
  }
}
```

### JAX-RS

JAXRSContract 使用 JAX-RS 规范重写覆盖了默认的注解处理。下面是一个使用 JAX-RS 的例子:

```java
interface GitHub {
  @GET @Path("/repos/{owner}/{repo}/contributors")
  List<Contributor> contributors(@PathParam("owner") String owner, @PathParam("repo") String repo);
}

public class Example {
  public static void main(String[] args) {
    GitHub github = Feign.builder()
                       .contract(new JAXRSContract())
                       .target(GitHub.class, "https://api.github.com");
  }
}
```

### OkHttp

OkHttpClient 使用 OkHttp 来发送 Feign 的请求，OkHttp 支持 SPDY (SPDY是Google开发的基于TCP的传输层协议，用以最小化网络延迟，提升网络速度，优化用户的网络使用体验),并有更好的控制http请求。

要让 Feign 使用 OkHttp ，你需要将 OkHttp 加入到你的环境变量中区，然后配置 Feign 使用 OkHttpClient，如下:

```java
public class Example {
  public static void main(String[] args) {
    GitHub github = Feign.builder()
                     .client(new OkHttpClient())
                     .target(GitHub.class, "https://api.github.com");
  }
}
```

### Ribbon

RibbonClient 重写了 Feign 客户端的对URL的处理，其添加了 智能路由以及一些其他由Ribbon提供的弹性功能。
集成Ribbon需要你将ribbon的客户端名称当做url的host部分来传递，如下：

```java
public class Example {
  public static void main(String[] args) {
    MyService api = Feign.builder()
          .client(RibbonClient.create())
          .target(MyService.class, "https://myAppProd");
  }
}
```

### Java 11 Http2

Http2Client将Feign的http请求定向到实现 HTTP/2的 Java11 New HTTP/2 Client。

要使用带有Feign的New HTTP/2 Client，请使用 Java SDK 11.然后，配置Feign以使用Http2Client：

```java
GitHub github = Feign.builder()
                     .client(new Http2Client())
                     .target(GitHub.class, "https://api.github.com");
```

### Hystrix

HystrixFeign 配置了 Hystrix 提供的熔断机制。
要在 Feign 中使用 Hystrix ，你需要添加Hystrix模块到你的环境变量，然后使用 HystrixFeign 来构造你的API:

```java
public class Example {
  public static void main(String[] args) {
    MyService api = HystrixFeign.builder().target(MyService.class, "https://myAppProd");
  }
}
```

### SOAP

SOAP包含可与XML API一起使用的编码器和解码器。

该模块增加了对通过JAXB和SOAPMessage编码和解码SOAP Body对象的支持。它还通过将SOAPFault解码功能包装到原始的javax.xml.ws.soap.SOAPFaultException中来提供SOAPFault解码功能，因此您只需捕获SOAPFaultException即可处理SOAPFault。

将SOAPEncoder 和/或 SOAPDecoder 添加到您的 Feign.Builder 中，如下所示：

```java
public class Example {
  public static void main(String[] args) {
    Api api = Feign.builder()
	     .encoder(new SOAPEncoder(jaxbFactory))
	     .decoder(new SOAPDecoder(jaxbFactory))
	     .errorDecoder(new SOAPErrorDecoder())
	     .target(MyApi.class, "http://api");
  }
}
```

NB: you may also need to add `SOAPErrorDecoder` if SOAP Faults are returned in response with error http codes (4xx, 5xx, ...)

### SLF4J

SLF4JModule 允许你使用 SLF4J 作为 Feign 的日志记录模块，这样你就可以轻松的使用 Logback, Log4J , 等来记录你的日志.

要在 Feign 中使用 SLF4J ，你需要添加SLF4J模块和对应的日志记录实现模块(比如Log4J)到你的环境变量，然后配置Feign使用Slf4jLogger :

```java
public class Example {
  public static void main(String[] args) {
    GitHub github = Feign.builder()
                     .logger(new Slf4jLogger())
                     .target(GitHub.class, "https://api.github.com");
  }
}
```

### Decoders

Feign.builder() 允许你自定义一些额外的配置，比如说如何解码一个响应。假如有接口方法返回的消息不是 Response, String, byte[] 或者 void 类型的，那么你需要配置一个非默认的解码器。

下面是一个配置使用JSON解码器(使用的是feign-gson扩展)的例子:

```java
public class Example {
  public static void main(String[] args) {
    GitHub github = Feign.builder()
                     .decoder(new GsonDecoder())
                     .target(GitHub.class, "https://api.github.com");
  }
}
```

假如你想在将响应传递给解码器处理前做一些额外的处理，那么你可以使用 mapAndDecode 方法。一个用例就是使用jsonp服务的时候:

```java
public class Example {
  public static void main(String[] args) {
    JsonpApi jsonpApi = Feign.builder()
                         .mapAndDecode((response, type) -> jsopUnwrap(response, type), new GsonDecoder())
                         .target(JsonpApi.class, "https://some-jsonp-api.com");
  }
}
```

### Encoders

发送一个Post请求最简单的方法就是传递一个 String 或者 byte[] 类型的参数了。你也许还需添加一个Content-Type请求头，如下:

```java
interface LoginClient {
  @RequestLine("POST /")
  @Headers("Content-Type: application/json")
  void login(String content);
}

public class Example {
  public static void main(String[] args) {
    client.login("{\"user_name\": \"denominator\", \"password\": \"secret\"}");
  }
}
```

通过配置一个解码器，你可以发送一个安全类型的请求体，如下是一个使用 feign-gson 扩展的例子:

```java
static class Credentials {
  final String user_name;
  final String password;

  Credentials(String user_name, String password) {
    this.user_name = user_name;
    this.password = password;
  }
}

interface LoginClient {
  @RequestLine("POST /")
  void login(Credentials creds);
}

public class Example {
  public static void main(String[] args) {
    LoginClient client = Feign.builder()
                              .encoder(new GsonEncoder())
                              .target(LoginClient.class, "https://foo.com");
    
    client.login(new Credentials("denominator", "secret"));
  }
}
```

### @Body templates

@Body注解申明一个请求体模板，模板中可以带有参数，与方法中 @Param 注解申明的参数相匹配,使用方法如下

```java
interface LoginClient {

  @RequestLine("POST /")
  @Headers("Content-Type: application/xml")
  @Body("<login \"user_name\"=\"{user_name}\" \"password\"=\"{password}\"/>")
  void xml(@Param("user_name") String user, @Param("password") String password);

  @RequestLine("POST /")
  @Headers("Content-Type: application/json")
  // json curly braces must be escaped!
  @Body("%7B\"user_name\": \"{user_name}\", \"password\": \"{password}\"%7D")
  void json(@Param("user_name") String user, @Param("password") String password);
}

public class Example {
  public static void main(String[] args) {
    client.xml("denominator", "secret"); // <login "user_name"="denominator" "password"="secret"/>
    client.json("denominator", "secret"); // {"user_name": "denominator", "password": "secret"}
  }
}
```

### Headers

Feign 支持给请求的api设置或者请求的客户端设置请求头

#### Set headers using apis

在特定接口或调用应始终设置某些标头值的情况下，它将标题定义为api的一部分是有意义的。

可以使用@Headers批注在api接口或方法上设置静态标头。

```java
@Headers("Accept: application/json")
interface BaseApi<V> {
  @Headers("Content-Type: application/json")
  @RequestLine("PUT /api/{key}")
  void put(@Param("key") String key, V value);
}
```

方法可以使用`@Headers`中的变量扩展为静态标头指定动态内容。

```java
public interface Api {
   @RequestLine("POST /")
   @Headers("X-Ping: {token}")
   void post(@Param("token") String token);
}
```

如果标题字段键和值都是动态的，并且可能的键的范围不能提前知道，并且可能在同一api /客户端中的不同方法调用之间变化（例如，自定义元数据头字段，如“x-amz-meta-\*”或“x-goog-meta-\*”），Map参数可以注释使用HeaderMap构造一个查询，该查询使用Map的内容作为其标题参数。

```java
public interface Api {
   @RequestLine("POST /")
   void post(@HeaderMap Map<String, Object> headerMap);
}
```

这些方法将标头条目指定为api的一部分，不需要任何自定义在构建Feign客户端时。

#### 给Target设置请求头
有时我们需要在一个API实现中根据不同的endpoint来传入不同的Header，这个时候我们可以使用自定义的RequestInterceptor 或 Target来实现.
通过自定义的 RequestInterceptor 来实现请查看 Request Interceptors 章节.
下面是一个通过自定义Target来实现给每个Target设置安全校验信息Header的例子:

```java
  static class DynamicAuthTokenTarget<T> implements Target<T> {
    public DynamicAuthTokenTarget(Class<T> clazz,
                                  UrlAndTokenProvider provider,
                                  ThreadLocal<String> requestIdProvider);
    
    @Override
    public Request apply(RequestTemplate input) {
      TokenIdAndPublicURL urlAndToken = provider.get();
      if (input.url().indexOf("http") != 0) {
        input.insert(0, urlAndToken.publicURL);
      }
      input.header("X-Auth-Token", urlAndToken.tokenId);
      input.header("X-Request-ID", requestIdProvider.get());

      return input.request();
    }
  }
  
  public class Example {
    public static void main(String[] args) {
      Bank bank = Feign.builder()
              .target(new DynamicAuthTokenTarget(Bank.class, provider, requestIdProvider));
    }
  }
```

这种方法的实现依赖于给Feign 客户端设置的自定义的RequestInterceptor 或 Target。可以被用来给一个客户端的所有api请求设置请求头。比如说可是被用来在header中设置身份校验信息。这些方法是在线程执行api请求的时候才会执行，所以是允许在运行时根据上下文来动态设置header的。
比如说可以根据线程本地存储(thread-local storage)来为不同的线程设置不同的请求头。

### Advanced usage

#### Base Apis
有些请求中的一些方法是通用的，但是可能会有不同的参数类型或者返回类型，这个时候可以这么用:
```java
interface BaseAPI {
  @RequestLine("GET /health")
  String health();

  @RequestLine("GET /all")
  List<Entity> all();
}
```

您可以定义和定位特定的api，继承基本方法

```java
interface CustomAPI extends BaseAPI {
  @RequestLine("GET /custom")
  String custom();
}
```

在许多情况下，资源表示也是一致的。因此，基本api接口支持类型参数。

```java
@Headers("Accept: application/json")
interface BaseApi<V> {

  @RequestLine("GET /api/{key}")
  V get(@Param("key") String key);

  @RequestLine("GET /api")
  List<V> list();

  @Headers("Content-Type: application/json")
  @RequestLine("PUT /api/{key}")
  void put(@Param("key") String key, V value);
}

interface FooApi extends BaseApi<Foo> { }

interface BarApi extends BaseApi<Bar> { }
```

#### Logging
你可以通过设置一个 Logger 来记录http消息，如下:
```java
public class Example {
  public static void main(String[] args) {
    GitHub github = Feign.builder()
                     .decoder(new GsonDecoder())
                     .logger(new Logger.JavaLogger().appendToFile("logs/http.log"))
                     .logLevel(Logger.Level.FULL)
                     .target(GitHub.class, "https://api.github.com");
  }
}
```

也可以参考上面的 SLF4J 章节的说明


#### Request Interceptors
当你希望修改所有的的请求的时候，你可以使用Request Interceptors。比如说，你作为一个中介，你可能需要为每个请求设置 X-Forwarded-For

```java
static class ForwardedForInterceptor implements RequestInterceptor {
  @Override public void apply(RequestTemplate template) {
    template.header("X-Forwarded-For", "origin.host.com");
  }
}

public class Example {
  public static void main(String[] args) {
    Bank bank = Feign.builder()
                 .decoder(accountDecoder)
                 .requestInterceptor(new ForwardedForInterceptor())
                 .target(Bank.class, "https://api.examplebank.com");
  }
}
```

或者，你可能需要实现Basic Auth，这里有一个内置的基础校验拦截器 BasicAuthRequestInterceptor

```java
public class Example {
  public static void main(String[] args) {
    Bank bank = Feign.builder()
                 .decoder(accountDecoder)
                 .requestInterceptor(new BasicAuthRequestInterceptor(username, password))
                 .target(Bank.class, "https://api.examplebank.com");
  }
}
```

#### Custom @Param Expansion

在使用 @Param 注解给模板中的参数设值的时候，默认的是使用的对象的 toString() 方法的值，通过声明 自定义的Param.Expander，用户可以控制其行为，比如说格式化 Date 类型的值:

```java
public interface Api {
  @RequestLine("GET /?since={date}") Result list(@Param(value = "date", expander = DateToMillis.class) Date date);
}
```

#### Dynamic Query Parameters

动态查询参数支持，通过使用 @QueryMap 可以允许动态传入请求参数,如下:

```java
public interface Api {
  @RequestLine("GET /find")
  V find(@QueryMap Map<String, Object> queryMap);
}
```

这也可以用于使用QueryMapEncoder从POJO对象生成查询参数。

```java
public interface Api {
  @RequestLine("GET /find")
  V find(@QueryMap CustomPojo customPojo);
}
```

以这种方式使用时，如果不指定自定义QueryMapEncoder，将使用成员变量名称作为查询参数名称生成查询映射。以下POJO将生成"/find?name={name}&number={number}"的查询参数（不保证包含的查询参数的顺序，并且通常，如果任何值为null，则将省略）。

```java
public class CustomPojo {
  private final String name;
  private final int number;

  public CustomPojo (String name, int number) {
    this.name = name;
    this.number = number;
  }
}
```

设置自定义的 `QueryMapEncoder`:

```java
public class Example {
  public static void main(String[] args) {
    MyApi myApi = Feign.builder()
                 .queryMapEncoder(new MyCustomQueryMapEncoder())
                 .target(MyApi.class, "https://api.hostname.com");
  }
}
```

使用@QueryMap注释对象时，默认编码器使用反射检查提供的对象字段以将对象值扩展为查询字符串。如果您更喜欢使用getter和setter方法构建查询字符串，如Java Beans API中所定义，请使用BeanQueryMapEncoder

```java
public class Example {
  public static void main(String[] args) {
    MyApi myApi = Feign.builder()
                 .queryMapEncoder(new BeanQueryMapEncoder())
                 .target(MyApi.class, "https://api.hostname.com");
  }
}
```

### Error Handling
如果您需要更多控制处理意外响应，Feign实例可以通过构建器注册自定义ErrorDecoder。

```java
public class Example {
  public static void main(String[] args) {
    MyApi myApi = Feign.builder()
                 .errorDecoder(new MyErrorDecoder())
                 .target(MyApi.class, "https://api.hostname.com");
  }
}
```

导致HTTP状态不在2xx范围内的所有响应都将触发ErrorDecoder的解码方法，允许您可以处理响应，将故障包装到自定义异常中或执行任何其他处理。如果要再次重试请求，请抛出RetryableException。这将调用已注册的Retryer。

### Retry
默认情况下，Feign将自动重试IOExceptions，无论HTTP方法如何，都将它们视为瞬态网络相关的异常，以及从ErrorDecoder抛出的任何RetryableException。要自定义这个行为，通过构建器注册自定义重试程序实例。

```java
public class Example {
  public static void main(String[] args) {
    MyApi myApi = Feign.builder()
                 .retryer(new MyRetryer())
                 .target(MyApi.class, "https://api.hostname.com");
  }
}
```

重试者负责通过返回true或者确定是否应该重试从方法continueOrPropagate（RetryableException e）中返回false; Retryer实例将是为每个客户端执行创建，允许您在需要时保持每个请求的状态。

如果确定重试失败，则抛出最后一次RetryException。扔掉原件导致重试失败的原因，使用exceptionPropagationPolicy（）选项构建您的Feign客户端。

#### Static and Default Methods

Feign所针对的接口可能具有静态或默认方法（如果使用Java 8+）。这些允许Feign客户端包含未由底层API明确定义的逻辑。例如，静态方法可以轻松指定常见的客户端构建配置;默认方法可用于组合查询或定义默认参数。

```java
interface GitHub {
  @RequestLine("GET /repos/{owner}/{repo}/contributors")
  List<Contributor> contributors(@Param("owner") String owner, @Param("repo") String repo);

  @RequestLine("GET /users/{username}/repos?sort={sort}")
  List<Repo> repos(@Param("username") String owner, @Param("sort") String sort);

  default List<Repo> repos(String owner) {
    return repos(owner, "full_name");
  }

  /**
   * Lists all contributors for all repos owned by a user.
   */
  default List<Contributor> contributors(String user) {
    MergingContributorList contributors = new MergingContributorList();
    for(Repo repo : this.repos(owner)) {
      contributors.addAll(this.contributors(user, repo.getName()));
    }
    return contributors.mergeResult();
  }

  static GitHub connect() {
    return Feign.builder()
                .decoder(new GsonDecoder())
                .target(GitHub.class, "https://api.github.com");
  }
}
```



