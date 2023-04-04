# 用 Resilience4j 实现弹簧云断路器

> 原文：<https://blog.devgenius.io/implementing-spring-cloud-circuit-breaker-with-resilience4j-cea647235cd7?source=collection_archive---------5----------------------->

不推荐使用 Spring Cloud Hystrix，这是新的选项

![](img/a78f544ecb1e2332d7ac4c438f4d4e74.png)

照片由[雷米·雅克因](https://unsplash.com/@jack_1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/old?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

最新的 Spring Cloud 版本不再支持 Spring Cloud Hystrix(工件-spring-Cloud-starter-网飞-hystrix)。它已被正式否决。作为 Hystrix 的替代品，引入了 Resilience4J。

在之前的一篇文章中，我们学习了使用 [Alibaba Sentinel](https://levelup.gitconnected.com/how-to-implement-a-circuit-breaker-in-microservice-with-alibaba-sentinel-d645f69b20ef) 实现断路器，这仍然是替代 Hystrix 的一个选项，但人们对 Resilience4J 项目的兴趣越来越大。

我们来看一下实现。

## 履行

与 Hystrix 和 Sentinel 不同，Resilience4J 只需最少的配置即可开始使用。

首先，让我们在 pom 文件中添加以下依赖项。

接下来，在我们打算处理系统故障的 Java 类中，让我们添加注释并提供一个回退方法。

像任何断路器一样，一旦 getStudentInfo()方法失败，Resilience4J 将调用 fallback 方法。

在上面的例子中，当通过 API 调用服务方法并且调用回退方法时，我们得到下面的输出

![](img/c75eddcf60ee8c235a12fdef8f0772cf.png)

## 结论

我更喜欢使用 Sentinel 而不是 Hystrix，因为 Sentinel 比 Hystrix 提供了更多的备选方案。

Resilience4J 提供了在返回回退选项之前重试失败方法一定次数的选项。它还带有其他功能，如速率限制器和舱壁，但我还没有使用它们。

在这三种实现中，Resilience4J 也是最快、最容易配置的。

这三个中你喜欢哪一个？请在评论中告诉我你的经历。