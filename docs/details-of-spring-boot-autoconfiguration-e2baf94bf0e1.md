# Spring Boot 自动配置的详细信息

> 原文：<https://blog.devgenius.io/details-of-spring-boot-autoconfiguration-e2baf94bf0e1?source=collection_archive---------17----------------------->

## 什么是 Spring Boot？Spring Boot 自动配置是如何工作的？

![](img/8d653de4049c6a22c74999e4cd75c5e0.png)

[活动创建者](https://unsplash.com/@campaign_creators?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 什么是 Spring Boot？

[Spring Boot](https://spring.io/projects/spring-boot) 网站上说“*我们对 Spring 平台和第三方库有自己的看法，所以你可以以最少的麻烦开始。大多数 Spring Boot 应用需要最小的弹簧配置。*

Spring Boot 是一个构建应用程序的框架。

Spring Boot 提供了不同的特性，如果您使用它来构建您的应用程序，您将需要不同的 beans。因此，自动配置通过创建这些 beans 来自动配置 Spring Boot 应用程序。

# 你为什么使用自动配置？

效率和时间。有了自动配置，Spring 将为开发人员做很多工作，并节省创建 beans 的时间。

幕后基本就是一堆`@Configuration`班。这些类不使用注释`@Configuration`。

这些类使用的一些注释是:

*   `@ConditionalOnClass` -只有当给定的类在类路径中时，应用程序才使用它。
*   `@Conditional` -仅在满足条件时
*   `@ConditionalOnMissingBean` -如果 bean 丢失或未创建，应用程序将使用此选项。

总之，`@Conditional`注释是所有注释的基础。

# 这个你到底是怎么理解的？

您或您的团队正在处理多个项目，并且这些项目共享一些公共代码。如果你想把这个公共代码提取到它自己的库或者共享 beans 中，那么所有的项目都可以使用它们。

```
@Configurationpublic class SharedObjects
{   
    @Bean   
    public CommonObject commonObject()   
    {      
       return new CommonObject();   
    }
}
```

一旦这个`CommonObject`通过 jar 文件共享，其他项目就可以导入它。

这种方法的缺点是，如果另一个项目想要使用`CommonObject`，但是不想使用该公共代码中的任何其他 beans。在项目启动期间，导入这些 beans 将是不必要的开销。因此，你需要一种方法来告诉 Spring，我们只需要`CommonObject` Bean 而不需要其他 Bean，甚至不要创建其他 Bean。这时我们就可以使用`@Conditional`注释了。

要使用这个`@Conditional`注释，有几种方法。Spring Boot 提供了一个类可以实现的`Condition`接口。

```
public class IsBrowserOnCondition implements Condition
{   
    @Override   
    public boolean matches(ConditionContext context, AnotatedTypeMetadata metadata)   
    {      
       return isMozillaFirefoxEnabled(context);   
    }          public boolean isMozillaFirefoxEnabled(ConditionContext context)   
    {      
       return context.getEnvironment().containsProperty("spring.preferredbrowser");   }
}
```

在这个类`IsBrowserOnCondition`中，我们看到了接口条件的实现。

*   这个实现包括方法`matches`。
*   此方法调用另一个方法来检查 Mozilla Firefox 浏览器是否已启用。
*   在这个过程中，它检查一个属性`spring.preferredbrowser`条件。
*   现在，如果我们想要在条件上创建新的 beans，我们将使用注释`@Conditional`作为`@Conditional(IsBrowserOnCondition.class)`。

简而言之，Spring Boot 是一个共享的上下文配置，包含许多使用注释`@Conditional`创建的 beans。

# 使用 Spring Boot 自动配置

为了更好地理解自动配置，我们将使用一个简单的 Spring Boot 应用程序。我们想知道当我们启动这个应用程序时会发生什么。

因此，该应用程序的主类如下所示:

当我运行这个主类时，Spring Boot 启动了 tomcat web 服务器。

在后台，Spring Boot 在 Tomcat 上启动应用程序时正在做一些工作。Spring Boot 在这里使用了 17 种不同的属性来源。官方文件 [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config) 提供了这 17 个来源的细节。开发人员可以将这些属性具体化，很多时候我们会为`application.properties`这样做。因此，如果您配置了这些属性中的任何一个，Spring Boot 将读取这些属性，而不是默认源。

现在，如果我们展开 jar 文件`spring-boot-autoconfigure-2.1.6.RELEASE.jar`，您将看到包`org.springframework.boot.autoconfigure`下的目录数量。所有这些子包都是 Spring Boot 提取的 beans，但是只是基于`@Conditional`注释使用它们。因此，在启动过程中，Spring Boot 会根据您在 Maven 或 Gradle 构建文件中配置的依赖项加载其中一些包。

从这个 jar 中，如果我们打开`ThymeleafAutoConfiguration`的源文件，我们将看到以下内容:

如果您正在构建一个 web 应用程序，百里香模板将是您的默认模板引擎。如果加载了`TemplateMode`和`SpringBootEngine`，Spring boot 将加载该类。我们可以看到`@Conditional`注释的用法。

# 如何排除 Spring Boot 自动配置？

Spring Boot 确实提供了一个选项来排除您不想包含在项目中的任何自动配置。

这里要记住的一点是，您必须知道为什么要排除某个 bean，如果您确定它可能会排除一些依赖配置。

# 结论

在这篇文章中，我展示了

*   Spring Boot 是如何工作的，以及如何使用一些依赖项来构建 Spring Boot 应用程序。
*   什么是自动配置，包括哪些内容。

如果你喜欢这篇文章或者有任何其他问题，请订阅[我的博客](https://betterjavacode.com/subscribe)。

*原载于 2020 年 6 月 21 日*[*【https://betterjavacode.com】*](https://betterjavacode.com/spring-boot/details-of-spring-boot-autoconfiguration)*。*