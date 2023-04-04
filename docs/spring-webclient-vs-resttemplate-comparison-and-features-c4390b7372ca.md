# Spring WebClient 与 rest template——比较和特性

> 原文：<https://blog.devgenius.io/spring-webclient-vs-resttemplate-comparison-and-features-c4390b7372ca?source=collection_archive---------1----------------------->

# 介绍

Spring 5 引入了一个新的反应式 web 客户端，叫做 web client。在这篇文章中，我将展示我们何时以及如何使用 Spring WebClient vs RestTemplate。我还将描述 WebClient 提供的特性。

# 什么是 RestTemplate？

RestTemplate 是一个中央 Spring 类，允许从客户端进行 HTTP 访问。`RestTemplate`提供 POST、GET、PUT、DELETE、HEAD 和 OPTIONS HTTP 方法。RestTemplate 的简单用例是消费 Restful web 服务。

您可以创建一个提供 RestTemplate 实例的 bean。然后，您可以在计划调用 REST 服务的任何类中`@autowire`这个 bean。RestTemplate 是实现接口`RestOperations`的类。

以下代码显示了 bean 的声明:

```
@Bean public RestOperations restOperations() 
{ 
  return new RestTemplate(); 
}
```

下面的代码显示了一个 REST 客户端“YelpClient”调用 Yelp 的 REST API 来获取租赁资产评论。

```
@Autowired 
private final RestOperations restOperations; public List getRentalPropertyReviews(String address) 
{ 
  String url = buildRestUrl(businessId); 
  HttpHeaders httpHeaders = new HttpHeaders(); 
  String apiKey = getApiKey(YELP); 
  httpHeaders.add("Authorization","Bearer " + apiKey); 
  httpHeaders.setContentType(MediaType.APPLICATION_JSON); 
  HttpEntity entity = new HttpEntity("parameters", httpHeaders); 
  ResponseEntity response; 
  try { 
   response = restOperations.exchange(url, HttpMethod.GET,entity, String.class); 
  } 
  catch(RestClientException e) 
  { 
    throw new RuntimeException("Unable to retrieve reviews", e); 
  } 
}
```

在上面的代码中，我们通过添加 Yelp 的 REST API 密钥作为授权的一部分来构建 HTTP 头。我们调用 GET 方法来获取评审数据。

基本上，一个人必须做

*   自动关联 RestTemplate 对象
*   使用授权和内容类型构建 HTTP 头
*   使用 HttpEntity 包装请求对象
*   提供 URL、Http 方法和交换方法的返回类型。

# 什么是 WebClient？

Spring 5 引入了一个名为 web client 的反应式 web 客户端。这是一个执行 web 请求的接口。它是 Spring web 反应模块的一部分。WebClient 最终会取代 RestTemplate。

最重要的是，WebClient 是反应式的、非阻塞的、异步的，并且基于 HTTP 协议 Http/1.1 工作。

要使用 WebClient，您必须

*   创建 WebClient 的实例
*   向 REST 端点发出请求
*   处理响应

```
WebClient webClient = WebClient .builder()  
     .baseUrl("https://localhost:8443") 
     .defaultCookie("cookieKey", "cookieValue") 
     .defaultHeader(HttpHeaders.CONTENT_TYPE, 
        MediaType.APPLICATION_JSON_VALUE) 
     .defaultUriVariables(Collections.singletonMap("url", "https://localhost:8443"))
     .build();
```

上面的代码显示了一种实例化 WebClient 的方法。您也可以通过简单地使用`WebClient webClient = WebClient.create();`来创建一个实例

`WebClient`提供了`exchange`和`retrieve`两种方法。方法通常获取响应以及状态和头。`retrieve`方法直接获取响应体。比较好用。

同样，根据您是试图获取单个响应对象还是一系列对象，您可以使用`mono`或`flux`。

```
this.webClient = webClientBuilder.baseUrl("http://localhost:8080/v1/betterjavacode/").build(); 
this.webClient.get() 
.uri("users") 
.accept(MediaType.APPLICATION_JSON) .retrieve()
.bodyToFlux(UserDto.class).collectList();
```

上面的代码基本上使用`webClient`从 REST API 获取用户列表。

# Spring WebClient vs RestTemplate

我们已经知道这两个特性之间的一个关键区别。WebClient 是非阻塞客户端，RestTemplate 是阻塞客户端。

RestTemplate 在幕后使用 Java Servlet API。Servlet API 是一个同步调用者。因为它是同步的，线程将阻塞，直到 webclient 响应请求。

因此，等待结果的请求将会增加。这将导致内存的增加。

另一方面，WebClient 是一个异步的非阻塞客户端。它使用 Spring 的反应框架。WebClient 是 Spring-WebFlux 模块的一部分。

Spring WebFlux 使用[反应器库](https://github.com/reactor/reactor)。它提供了`Mono`和`Flux` API 来处理数据序列。Reactor 是一个反应流库。而且，它的所有操作器都支持非阻塞背压。

# 如何在 Spring Boot 应用程序中使用 WebClient 的示例

我们可以结合 Spring Web MVC 和 Spring WebFlux 的功能。在本节中，我将创建一个示例应用程序。这个应用程序将使用 WebFlux 调用 REST API，我们将构建一个响应来显示一个包含用户列表的网页。

`RestController`举这个例子是一个获取用户列表的 API:

```
package com.betterjavacode.webclientdemo.controllers; import com.betterjavacode.webclientdemo.dto.UserDto; 
import com.betterjavacode.webclientdemo.managers.UserManager; 
import org.springframework.beans.factory.annotation.Autowired; import org.springframework.web.bind.annotation.GetMapping; 
import org.springframework.web.bind.annotation.RequestMapping; import org.springframework.web.bind.annotation.RestController; import java.util.List; @RestController 
@RequestMapping("v1/betterjavacode") 
public class UserController 
{    @Autowired 
   public UserManager userManager;    @GetMapping(value = "/users") 
   public List getUsers() 
   { 
     return userManager.getAllUsers(); 
   } 
}
```

`Controller`使用 WebClient 调用 REST API 的类如下所示:

```
package com.betterjavacode.webclientdemo.controllers; import com.betterjavacode.webclientdemo.clients.UserClient; 
import com.betterjavacode.webclientdemo.dto.UserDto; 
import com.betterjavacode.webclientdemo.managers.UserManager; 
import org.springframework.beans.factory.annotation.Autowired; import org.springframework.stereotype.Controller; 
import org.springframework.ui.Model; 
import org.springframework.web.bind.annotation.GetMapping; 
import java.util.List; @Controller 
public class MainController 
{    @Autowired 
   UserClient userClient;    @GetMapping(value = "/") 
   public String home() 
   { 
     return "home"; 
   }    @GetMapping(value = "/users") 
   public String getUsers(Model model) 
   { 
     List users = userClient.getUsers().block(); 
     model.addAttribute("userslist", users); return "users"; 
   } 
}
```

现在，UserClient 的重要代码是我们将使用 WebClient 调用 REST API 的地方。

```
package com.betterjavacode.webclientdemo.clients; import com.betterjavacode.webclientdemo.dto.UserDto; 
import org.springframework.http.MediaType; 
import org.springframework.stereotype.Service; 
import org.springframework.web.reactive.function.client.WebClient; import reactor.core.publisher.Flux; 
import reactor.core.publisher.Mono; 
import java.util.List; @Service 
public class UserClient 
{   private WebClient webClient; 
  public UserClient(WebClient.Builder webClientBuilder) 
  { 
     this.webClient = webClientBuilder.baseUrl("http://localhost:8080/v1/betterjavacode/").build(); 
  }    public Mono<List> getUsers() 
   { 
     return this.webClient.get() 
            .uri("users") 
            .accept(MediaType.APPLICATION_JSON) 
            .retrieve().bodyToFlux(UserDto.class).collectList(); 
   } 
}
```

上面的代码显示了首先构建 WebClient，然后使用它从 REST API 中检索响应。`retrieve`方法提供`mono`或`flux`两种选择。因为我们有不止一个用户要获取，所以我们使用了`flux`。

这表明我们可以使用反应式、非阻塞的 WebClient，它是 Spring Web MVC 框架中 WebFlux 的一部分。

# Spring WebClient 还有什么？

Spring WebClient 是`Spring WebFlux`框架的一部分。这个 API 的主要优点是开发人员不必担心并发性或线程。WebClient 负责这一点。

WebClient 有一个内置的 HTTP 客户端库支持来执行请求。这包括阿帕奇`HttpComponents`，码头反应`HttpClient`，或反应器`Netty`。

`WebClient.builder()`提供以下选项:

*   `uriBuilderFactory` -定制`uriBuilderFactory`以使用基本 URL
*   `defaultHeader` -每个请求的标题
*   `defaultCookie` -针对每个请求的 Cookies
*   `defaultRequest` -定制每个请求
*   `filter` -针对每个请求的客户端过滤器
*   `exchangeStrategies` - HTTP 消息阅读器/编写器定制

我已经在上面的代码演示中展示了`retrieve`方法。

`WebClient`也提供了一种方法`exchange`，有`exchangeToMono`和`exchangeToFlux`这样的变种。

使用`attribute()`，我们还可以向请求添加属性。

或者，也可以使用`WebClient`进行同步使用。在我上面的例子`MainController`中，我使用`block`来获得最终结果。这基本上阻塞了并行调用，直到我们得到结果。

`WebClient`提供的一个关键特性是`retryWhen()`。对于更具弹性的系统，这是一个很好的特性，您可以在使用`WebClient`时添加。

```
webClient 
.get() 
.uri(String.join("", "/users", id)) 
.retrieve() 
.bodyToMono(UserDto.class) 
.retryWhen(Retry.fixedDelay(5, Duration.ofMillis(100))) 
.block();
```

`retryWhen`将`Retry`类作为参数。

`WebClient`还提供了错误处理功能。`doOnError()`允许您处理错误。当单声道因错误而结束时，它被触发。`onErrorResume()`是基于错误的回退。

# 结论

在这篇文章中，我展示了什么是 Spring WebClient，我们如何使用 Spring WebClient 和 RestTemplate，以及它提供了哪些不同的特性。

如果你喜欢这篇文章，你可以在这里订阅我的博客[。](https://betterjavacode.com/subscribe)

# 参考

1.  Spring WebClient — [Spring 文档](https://docs.spring.io/spring-framework/docs/current/reference/html/web-reactive.html#webflux-client)
2.  WebClient 备忘单— [Spring WebClient](https://medium.com/swlh/spring-boot-webclient-cheat-sheet-5be26cfa3e)

*原载于 2020 年 12 月 27 日 https://betterjavacode.com**[*。*](https://betterjavacode.com/programming/spring-webclient-vs-resttemplate-comparison-and-features)*