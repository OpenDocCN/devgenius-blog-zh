# 简化的弹簧云注释

> 原文：<https://blog.devgenius.io/spring-cloud-useful-annotations-and-explanation-that-are-worth-knowing-for-developing-microservices-4e0ec2339062?source=collection_archive---------1----------------------->

## 春云|微服务

## Spring Cloud 有很有用的注解，值得开发微服务的人了解。

![](img/b78d0c1c35d8bbe76e6a831b393a1f80.png)

背景图片由[米克·德·保拉](https://unsplash.com/@themick79i?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/cloud-computing?  utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Spring Cloud module 是原始 Spring 框架的一个巨大扩展，它为开发人员提供了在分布式系统中快速构建一些常见模式的工具，例如服务发现、断路器、智能路由、控制总线、令牌等。

就像 Spring Boot 一样，Spring Cloud 也有一些有用的注释，值得 Spring Cloud 开发人员了解。

## EnableConfigServer

使用@EnableConfigServer

**@EnableConfigServer** 是类级注释。它帮助 Spring Cloud Config Server 非常容易地嵌入到 Spring Boot 应用程序中，并且**@ EnableConfigServer***注释使 Spring Boot 应用程序充当配置服务器。*

*![](img/2dd2ac32cef79e35ba55139f65a2983c.png)*

*尤里卡*

## ***EnableEurekaServer***

*在大型应用程序中，我们可能有许多应该协同工作的微服务。这些服务可能有不同的服务器地址。所以现在我们面临一个严重的问题，那就是服务之间如何对话。一个简单的方法可能是在使用 RestTemplate 或 WebClient 调用时硬编码所有服务地址，包括端口。*

*但是它认为这是一种不好的做法，因为一个服务可能有多个使用不同地址运行的实例，所以在这种情况下，我们如何知道要连接哪些实例。*

*为了解决这类具体问题，我们可以通过 **Eureka** 使用 [**服务发现**](/service-discovery-importance-in-microservices-17970569685) 。*

*[](/service-discovery-importance-in-microservices-17970569685) [## 什么是服务发现&为什么它在微服务中很重要？

### 什么是服务发现及其在微服务中的重要性

blog.devgenius.io](/service-discovery-importance-in-microservices-17970569685) 

@EnableEurekaServer 的使用

我们可以通过用 **@EnableEurekaServer *注释一个类，将 Spring Boot 应用程序转换成 **Eureka** 服务器。现在我们可以使用这个服务器来查看其他的 Eureka 客户端服务来管理它们。服务必须注册到此服务器。***

## EnableEurekaClient

如果服务/客户端没有用来自`spring-cloud-netflix` ***的**@ EnableEurekaClient***进行注释，尤里卡服务器就无法找到其他服务/客户端。**** *我们要向服务器注册的每个服务都要用**@ EnableEurekaClient***注释。它是类级别的注释。**

**@EnableEurekaClient 的使用**

## ****启用发现客户端****

**来自`spring-cloud-commons`的***@ EnableDiscoveryClient***与***@ EnableEurekaClient***相同，但它是“发现服务”的更通用的实现。**

*****@ enableeureka client***仅适用于尤里卡，而***@ EnableDiscoveryClient***适用于**尤里卡**、**领事**、**动物园管理员**。但是如果 Eureka 在**应用程序类路径**上，它们实际上就是**相同的**。**

## **启用电路断路器**

**假设在电影租赁应用程序中，我们有三个微服务:MovieCatalogService、MovieRatingService 和 MovieOrderService。**

**@EnableCircuitBreaker 的用法**

**并且，这些微服务需要相互交互来处理用户的请求。例如，MovieOrderService 可以调用 MovieCatalogService 和 MovieRatingService 来显示电影名称(来自 MovieCatalogService)和电影分级(来自 MovieRatingService)以及在电影租赁过程中向用户显示的订单(来自 MovieOrderService)。**

**假设由于某种原因(网络错误或过载), MovieCatalogService 完全失败并停止为请求提供服务。因此，整个生态系统将由于这种单点故障而变得不稳定。**

**在这种情况下，断路器模式就派上了用场。一旦发现任何此类情况，它会将流量重定向到回退路径。**@ EnableCircuitBreaker***是一个类级别的注释，我们需要将这个注释应用到您的应用程序的每一个服务中。***

***有许多工具([网飞 Hystrix](https://github.com/Netflix/Hystrix) 、 [Resilience4J](https://github.com/resilience4j/resilience4j) 、 [Sentinel](https://github.com/alibaba/Sentinel) 、 [Spring Retry](https://github.com/spring-projects/spring-retry) )可用于在春季实现断路器模式。但在本帖中，我们将讨论[网飞·海斯特里克斯](https://github.com/Netflix/Hystrix)的常见注释。***

## *****EnableHystrix&EnableHystrix dashboard*****

***@EnableHystrixDashboard 的使用***

*****@EnableHystrix** 是可选的。**@ EnableCircuitBreaker***将在类路径中扫描任何兼容的断路器实现，如果 **Hystrix** 在类路径中，那么我们不需要显式启用 **Hystrix。******

*****@ EnableHystrixDashboard**增加了一个有用的仪表板，运行在 Hystrix 提供的 localhost 上，用于监控其状态。***

## ***hystrix 命令***

***@ hystrix 命令的使用***

***我们启用了具有相同签名的回退方法**@ hystrix command(fallback method = " getMovieOrdersFallback ")**。现在，如果实际的电影订购服务由于某种原因关闭，将调用 fallback 方法。***

## ***负载平衡***

***使用@LoadBalanced***

******@ load balanced***是一个标记标注。用于指示 **RestTemplate** 或 **WebClient** 应该使用**RibbonLoadBalancerClient**或 **LoadBalancerClient** 与服务进行交互。反过来，这允许你为我们传递给 **RestTemplate** 或 **WebClient** 的 URL 使用“逻辑标识符”。这些逻辑标识符通常是在服务发现中注册的服务的名称。***

***注*:-**@ ribbon client(**spring-cloud-starter-网飞-ribbon **)** 和**@ LoadBalancerClient**(spring-cloud-commons)可选。这既可以作为客户端的负载平衡器，又可以让我们控制 HTTP 和 TCP 客户端。如果我们使用服务发现，那么我们可以使用默认设置。我们不需要显式地使用这些注释。**

**@ ribbon client/@ LoadBalancerClient 的使用**

**但是如果我们没有使用任何服务发现或者需要为特定的客户端定制 **Ribbon** 或 **LoadBalancer** 的设置，那么我们需要使用这两个中的任何一个。**

*   **`name` -我们称之为 Ribbon 或 LoadBlancer 的服务，但需要对 Ribbon 或 LoadBlancer 如何与该服务交互进行额外的定制。**
*   **`configuration`——将它设置为一个`@Configuration`类，我们所有的定制都定义为`@Beans .`**

**这个帖子对你有帮助吗？不要忘记为这个帖子鼓掌，给我们一些灵感！*** 

# **你对全栈开发感兴趣吗？**

***我写的是关于 fullstack Web Development REST API、微服务、LinkedIn 中的架构、Medium 的循序渐进的编码教程。* [*这里是我的 Linkedin 简介*](https://www.linkedin.com/in/spsarkar-appxive/) *。***