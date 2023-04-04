# Spring Boot 安全指南

> 原文：<https://blog.devgenius.io/a-guide-to-spring-boot-security-ca50679c0c49?source=collection_archive---------1----------------------->

![](img/8618861222ac414940d262b71cf44092.png)

照片由[约伯·摩西](https://unsplash.com/@security_concepts_services?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 概观

在本文中，我们将了解如何向我们的 Spring Boot 项目添加安全配置。我们将学习 Spring 安全默认行为是如何工作的，然后我们将提供我们自己的配置。

# 术语

在进入实现部分之前，最好先理解这些常用术语。

*   证明

身份验证是验证您的登录凭据的过程。它在验证你是谁。

*   批准

授权是认证之后的下一步。它是验证您可以访问某些内容的过程。基本上它意味着你被允许做什么。

*   授予的权力

在 Spring Security 中，我们可以将每个被授予的权限看作一个单独的特权。例子可以包括读权限、写权限等。如您所见，我们也可以通过使用特权来引用权威的概念。

# Spring Boot 证券违约行为

为了给 Spring Boot 应用程序添加安全性，您需要添加安全启动程序依赖项，如下所示:

Spring Security 中有一些预定义的属性，

如果您没有使用预定义的属性*spring . security . user . password*配置密码并启动应用程序，您会注意到一个默认密码被随机生成并打印在控制台日志中:

```
Using generated security password: 5c927b54-1a7e-4291-9dff-03965e0dca98
```

Spring 安全默认行为在您的项目中做了以下事情，

*   为 URL 添加强制身份验证
*   添加登录表单
*   处理登录错误

# 禁用自动配置

要放弃安全自动配置并添加我们自己的配置，我们需要排除*SecurityAutoConfiguration*类。

这可以通过简单的排除来实现

或者通过在 *application.properties* 文件中添加一些配置:

# 提供定制配置

当我们禁用安全自动配置时，我们需要提供我们自己的配置。让我们试着理解下面的代码段发生了什么。@EnableWebSecurity 允许 spring 找到并自动将该类应用到全局 WebSecurity。WebSecurityConfigurerAdapter 允许配置影响所有 web 安全的东西。我们必须使用 AuthenticationManagerBuilder，以便可以在其上设置配置。我们已经将其配置为内存中身份验证。然后我们需要设置用户、密码和角色。这里我们配置了两个用户，一个是用户，另一个是管理员。

当我们设置密码时，使用散列密码是很重要的。为此，我们创建了一个 PasswordEncoder 类型的 bean。然后，我们必须在应用程序中配置授权。当我们扩展 WebSecurityConfigurerAdapter 时，它会覆盖它的一些方法来设置 web 安全配置的一些细节。configure(HttpSecurity)方法定义了哪些 URL 路径应该受到保护，哪些不应该。

我想这总结了我的文章，其中包括 Spring Boot 安全的主要概念。要了解更多关于 Spring Boot 安全的信息，请通过***s***[***spring Security***](https://www.baeldung.com/spring-boot-security-autoconfiguration)***。*** 有很多像 [Baeldung](https://www.baeldung.com/security-spring) 和 LinkedIn learning 这样的解决方案，根据自己的水平来学习。我鼓励检查它们，因为保护您的应用程序是要考虑的最重要的事情之一。干杯！！！