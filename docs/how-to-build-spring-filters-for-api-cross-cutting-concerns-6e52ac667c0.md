# 如何为 API 横切关注点构建 Spring 过滤器

> 原文：<https://blog.devgenius.io/how-to-build-spring-filters-for-api-cross-cutting-concerns-6e52ac667c0?source=collection_archive---------1----------------------->

## 关于过滤器你需要知道的一切——定义、URL 模式、排序、单元测试

![](img/d8ebac8a8d02d8008e8ec320c2f1bc40.png)

斯蒂芬·克拉克莫在 Unsplash[拍摄的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

API 不仅限于业务功能，还包括横切关注点的公共逻辑，比如审计日志和安全验证。在 Java Spring Boot 中，审计日志和安全检查可以添加到定义 API 端点的控制器处的系统逻辑的开头。但是，您很快就会发现，向每个端点添加相同的逻辑集是一件很麻烦的事情，这导致在需要对安全逻辑进行任何更改的情况下，需要花费相当大的精力来维护源代码。

Spring 框架提供了一个更好的解决方案，它允许我们将公共系统逻辑放入控制器前面的过滤器链中。由于过滤器是在请求通过过滤器链时按顺序执行的，因此有可能在进一步处理之前拒绝某些请求。例如，IP 白名单可以阻止来自未授权 IP 地址的请求，而无需运行任何身份验证逻辑。

![](img/6148d78ed874cfb14c6e2daf4ed55e06.png)

弹簧过滤器链

在本文中，我将向您展示如何使用审计日志和 IP 白名单为 REST APIs 构建以下典型的过滤器链。

![](img/aa1c3c9de88aa67c247750ad81129b47.png)

示例应用程序

# 如何定义过滤器？

Spring 框架自动识别任何被定义为 bean 的过滤器或过滤器注册。过滤器实现 Servlet 过滤器接口，过滤器日志驻留在`doFilter()`方法中。下面的示例过滤器记录了所有请求和响应。

这个请求日志过滤器只是记录请求，将请求传递给下一个过滤器，然后记录响应。

由于过滤器被定义为带有注释的 bean，Spring 框架会自动注册过滤器。

正如您在下面看到的示例日志，过滤器记录了请求和响应。

```
2021-12-19 11:03:53 INFO s.g.forex.filters.RequestLogFilter.doFilter - [Request] GET /rates/latest/GBP2021-12-19 11:03:59 INFO s.g.forex.filters.RequestLogFilter.doFilter - [Response] GET /rates/latest/GBP 200 OK2021-12-19 11:04:13 INFO s.g.forex.filters.RequestLogFilter.doFilter - [Request] POST /rates/book2021-12-19 11:04:13 INFO s.g.forex.filters.RequestLogFilter.doFilter - [Response] POST /rates/book 200 OK
```

# 特定 URL 模式的过滤器

默认情况下，过滤 bean 应用于所有请求，而不考虑 URL 路径。对于某些用例，您可能希望将过滤器逻辑应用于特定的 API。例如，检索最新外汇汇率的 API 对任何人开放，而只有授权客户才能调用外汇交易的 API。

为此，请定义过滤器注册 bean，而不是只定义过滤器 bean。过滤器注册包含附加信息，如 URL 模式。然后，框架将只对特定的 URL 模式应用过滤器。以下示例将 IP 白名单过滤器注册到路径 `/deals` 中，这是外汇交易 API 的 URL 路径。换句话说，URL 路径`/rates`上的费率查询等其他 API 是公开使用的。

默认情况下，Spring 框架使用 URL 模式`/*`注册过滤器 bean，将过滤器应用于所有请求。因为只有 URL 模式`/deals*`受到限制，所以我们不需要注释`@Component` 类来避免自动注册。如果客户端 IP 地址在授权的 IP 范围之外，则过滤器会将未授权 403 设置为响应状态代码。它不会将请求传递给过滤器链，而是会立即返回，因为进一步处理未授权的请求是没有意义的。

# 过滤器排序

无论客户端 IP 地址如何，都需要审计日志记录。因此，确保为请求日志筛选器分配比 IP 地址筛选器更高的优先级非常重要。否则，系统将无法记录带有未授权 IP 地址的请求。换句话说，请求日志筛选器应该位于筛选器链中 IP 地址筛选器之前的位置。

![](img/81ba8f420cf42a8c416a7b9fdddf9612.png)

优先过滤器

默认情况下，如果定义的过滤器没有排序信息，Spring 框架会注册优先级最低的过滤器。要指定过滤器顺序，使用注释`@Order`是用顺序声明过滤器 bean 的最直接的方法。以下示例显示，最高优先级被分配给具有常量值`HIGHEST_PRECEDENCE` 的请求日志过滤器，该常量值为`Integer.***MIN_VALUE.***` 。分配给 order 的值越低，优先级越高。

或者，为注册 bean 配置订单值。将所有过滤器的设置集中在一个配置中更清晰、更易于维护。

# 过滤器的单元测试

最后，过滤器上的单元测试与其他组件上的测试是一样的。技术是隔离过滤器和模仿所有的依赖关系。为了验证 IP 地址过滤器的行为，为授权的 IP 地址的配置设置 mock，然后用一个 mock 请求直接调用过滤器，后面是输出上的断言。

![](img/ded7e3bf4eed2f683bebd88717c5eb76.png)

IP 地址过滤器上的单元测试设置

下面的示例代码利用`MockHttpServletRequest`和`MockHttpServletResponse`分别模拟请求和响应。用客户机 IP 地址构建一个请求，并调用 IP 地址过滤器的`doFilter()`方法。接下来，检查响应的状态代码。

下面的示例代码演示了分别具有有效 IP 地址和授权 IP 地址的单元测试用例。

# 最后的想法

当涉及到所有请求的公共系统逻辑时，过滤器是一个有用的工具。Spring 框架极大地简化了过滤器配置，因为它可以自动注册任何过滤器 beans。如果需要定制，比如 URL 模式，那么`FilterRegistrationBean`是过滤器配置的便利对象。由于过滤器是在一个链中执行的，请求和响应上的更新可能会影响链中所有后续过滤器的行为。因此，作为设计的一部分，了解滤波器的执行顺序至关重要。

如果您对示例应用程序的实现感兴趣，可以从 GitHub 获得完整的源代码:

[](https://github.com/gavinklfong/servlet-spring-forex-trade) [## GitHub-gavinklfong/servlet-spring-forex-trade

### 这个库是一个克隆的反应春季外汇交易[https://github.com/gavinklfong/reactive-spring-forex-trade…

github.com](https://github.com/gavinklfong/servlet-spring-forex-trade)