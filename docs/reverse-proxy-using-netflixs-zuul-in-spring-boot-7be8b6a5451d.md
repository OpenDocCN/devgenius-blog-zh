# 在 Spring Boot 使用网飞 Zuul 的反向代理

> 原文：<https://blog.devgenius.io/reverse-proxy-using-netflixs-zuul-in-spring-boot-7be8b6a5451d?source=collection_archive---------0----------------------->

在本文中，我们将发现什么是反向代理，以及我们如何使用网飞的 Zuul 在 Spring boot 中为一个推荐网站实现反向代理。

# 定义

`define: reverse proxy`

> 在计算机网络中，**反向代理**是位于后端应用程序前面的应用程序，它将客户端请求转发给这些应用程序。反向代理有助于提高可扩展性、性能、弹性和安全性。返回给客户端的资源看起来好像是来自反向代理本身

![](img/5fd6585407df4c8b8348d5b6217b716a.png)

反向代理图

# 反向代理的使用

*   **负载均衡:**它可以将来自传入请求的负载分配给几个服务器，每个服务器支持自己的应用领域。
*   **安全性:**它可以通过消除对后端服务器的直接互联网访问来隐藏后端服务器的拓扑结构和特征。
*   **认证:**它可以用来为所有 HTTP 请求提供单点认证。
*   **SSL 终端:**它可以处理传入的 HTTPS 连接，解密请求并将未加密的请求传递到服务器上。
*   **提供静态内容:**它还可以充当提供静态内容的 web 服务器。
*   **缓存:**可以充当缓存。
*   **压缩:**为了减少单个请求所需的带宽，它可以对传入的请求进行解压缩，对传出的请求进行压缩。
*   **集中式日志记录和审计:**由于所有的 HTTP 请求都通过反向代理路由，这为日志记录和审计提供了一个很好的切入点。
*   **URL 重写:** It 可以在将 URL 传递给后端服务器之前重写它们。
*   **将多个网站聚合到同一个 URL 空间:**它可以将单个 URL 地址空间的不同分支路由到不同的内部 web 服务器

看看这篇关于反向代理的不同用例的博客。

# 履行

在这一节中，我们将使用 Spring Boot 的 Spring Cloud Starter 网飞祖尔来为我们的推荐网站创建一个反向代理。

假设我们有不同的电影推荐、电视剧推荐、书籍推荐和音乐推荐应用。但是我们希望在同一个父 URL 下托管所有这些应用程序，比如说`/recommendation/**`。

## 属国

```
<!--[https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-zuul](https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-zuul) 
-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
    <version>2.2.10.RELEASE</version>
</dependency>
```

## 密码

首先，我们将在我们的`Application.java`中启用 Zuul 反向代理。

```
@EnableZuulProxy
@SpringBootApplication
public class RoutingAndFilteringGatewayApplication {

  public static void main(String[] args) {
      SpringApplication.run(
         RoutingAndFilteringGatewayApplication.class, args
      );
  }
}
```

然后，在我们的控制器中，我们将有业务逻辑来找出应该将请求转发给哪个应用程序，并将它转发给那个应用程序。

```
@Slf4j
@Controller
public class RecommendationController { /**
     * Main handler
     * @param type              Type of recommendation
     * @param name              Name
     * @return {String}         forwarded URL
     */
    @GetMapping(value = { "/recommendation/{type}/{name}"})
    public String main( 
        @PathVariable("type") String type,
        @PathVariable("name") String name
    ){
         // Here type can be 
         // 1\. movie
         // 2\. tv
         // 3\. book
         // 4\. music
        type = type.toLowerCase();
        log.info("type: " + type);
        return "forward:/" + type + "/" + name;
    }
}
```

现在，进入主要部分，我们将在`application.yml`中定义 Zuul 属性

```
zuul:
  routes:
    movie:
      path: /movie/**
      url: [http://localhost:3001/movie/](http://localhost:3001/movie/)
    tv:
      path: /tv/**
      url: [http://localhost:3002/tv/](http://localhost:3002/tv/)
    book:
      path: /book/**
      url: [http://localhost:3003/book/](http://localhost:3003/book/)
    music:
      path: /music/**
      url: [http://localhost:3004/music/](http://localhost:3001/music/)
```

这样，所有以`/movie/**`开头的请求都将被路由到`http://localhost:3001/movie`，对于电视、书籍等等也是如此。

在我们基于 path 变量的推荐控制器中，我们将请求转发给这些应用程序之一。

## 奖金代码

这不是我们的用例所需要的。但是，假设您必须拦截请求，并添加一些请求头，添加一些日志记录等。你甚至可以通过添加 Zuul 滤镜来做这些事情。有各种类型过滤器，如前置过滤器、路由过滤器、后置过滤器和错误过滤器。

在这一节中，我将向您展示如何使用“pre”过滤器在请求中添加定制的请求头。

```
@Component
public class SimpleFilter extends ZuulFilter {

    @Override
    public String filterType() {
        return "pre";
    }

    @Override
    public int filterOrder() {
        return 1;
    }

    @Override
    public boolean shouldFilter() {
        return true;
    }

    @Override
    public Object run() {
        RequestContext ctx = RequestContext.getCurrentContext();
        HttpServletRequest request = ctx.getRequest();
        String requestURL = request.getRequestURL().toString();

        // Here we are adding a request header "audio-visual: yes"
        // If the request URL contains /movie/ or /tv/
        if (requestURL.contains("/movie/") ||
            requestURL.contains("/tv/")
        ) {
            ctx.addZuulRequestHeader("audio-visual", "yes");
        }

        return null;
    }}
```

如果你想了解更多。请检查这些文件。

1.  【https://en.wikipedia.org/wiki/Reverse_proxy 
2.  [https://dzone.com/articles/benefits-reverse-proxy](https://dzone.com/articles/benefits-reverse-proxy)
3.  [https://www.baeldung.com/spring-rest-with-zuul-proxy](https://www.baeldung.com/spring-rest-with-zuul-proxy)
4.  [https://cloud . spring . io/spring-cloud-网飞/multi/multi _ _ router _ and _ filter _ zuul . html](https://cloud.spring.io/spring-cloud-netflix/multi/multi__router_and_filter_zuul.html)
5.  [https://github.com/Netflix/zuul](https://github.com/Netflix/zuul)