# 每个开发人员都必须知道的关于 Spring Boot 的特性

> 原文：<https://blog.devgenius.io/features-that-every-developer-must-know-about-spring-boot-c1c0d7f1c0a8?source=collection_archive---------0----------------------->

## 该框架通常被称为“类固醇上的弹簧”，的确如此。

![](img/7ad795d18ff8258d5d2267697b126dc8.png)

照片由[上的](https://unsplash.com/s/photos/programmer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)[砌墙](https://unsplash.com/@walling?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄

如果你不是生活在岩石下，那么你一定听说过 Spring Boot，这个框架提供了一种更简单、更快速的方法来设置、配置和运行简单的和基于 web 的应用程序。Spring Boot 是一个[框架，由 Pivotal 团队创建，用于简化新 Spring 应用](https://spring.io/projects/spring-boot)的引导和开发。

# 历史

嗯，Pivotal 因为严重依赖基于 XML 的配置而备受批评。2013 年，[Pivotal 的 CTO 将公司的使命定为开发一个无 XML 的开发平台](https://www.youtube.com/watch?v=jplkJIHPGos)，该平台不仅可以简化应用程序的开发，还可以简化依赖关系管理，这在当时是一场噩梦。

2013 年第三季度，Spring Boot 用一条不超过 140 个字符的可运行 web 应用程序展示了它的简单性，赢得了巨大的人气。这条推文是在它的第一个测试版发布后发布的，足以让开发者谈论它。

2013 年，Spring Boot 的“你好，世界”在推特上疯传

Spring Boot，在单行中表示:
**( Spring Framework — XML 配置)+集成服务器**

# Spring Boot 的一些组成部分

**Spring Boot 核心**是其他 Spring 模型的基础，并提供独立工作的验证功能。 **Spring Boot CLI** 其功能是基于 ruby 和 rails 的命令行界面。但是，要启动和停止应用程序，需要 spring boot。

**Spring Boot 执行器**启用可在您的应用中使用的企业功能，它可以自动检测您的应用的框架和功能，并在需要时相应地使用它。通过在 *pom.xml* 文件中包含***spring-boot-starter-actuator***starter，可以将执行器与 spring boot 应用程序集成在一起；

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**Spring Boot 启动器**帮助启动项目，并作为一个依赖项包含在您构建的文件中。它自动为您正在开发的应用程序类型添加启动项目和依赖项。我们有 50 多名首发队员。最常用的有:

*   ***spring-boot-starter****:*core starter，包括自动配置支持、日志、YAML
*   ***spring-boot-starter-Data-jpa:***starter 用于将 Spring 数据 JPA 与 Hibernate 结合使用
*   ***Spring-boot-starter-Security:***starter 用于使用 Spring Security
*   ***spring-boot-starter-test:***用于测试 Spring Boot 应用的启动器
*   ***Spring-boot-starter-web:***使用 Spring MVC 构建 web，包括 RESTful，应用程序的启动器

# Spring Boot 的特色

有大量的特性被证明可以让开发人员的生活变得更容易。然而，下面的特性是最重要的。

## 依赖性管理

在 spring-boot 框架发布之前，依赖关系管理是一项非常艰巨的任务，尤其是对于新手开发人员，甚至是经验丰富的开发人员来说，因为他们需要了解兼容的依赖关系，才能让应用程序正常运行。

Spring Boot 自动管理依赖关系和配置。Spring Boot 的每个版本都提供了它所支持的依赖项列表。依赖项列表作为**物料清单**或 **BOM** 的一部分提供，本质上是可以与 **Maven** 一起使用的弹簧启动依赖项。这意味着您不一定需要提及依赖项的版本，因为 Spring 会管理它。这避免了不同版本的 Spring Boot 库的不匹配，如果你正在处理一个多模块的项目，这是非常有用的。

## 自动配置

如果你问我，这就是*Spring Boot*最重要的特点*是自动配置*。它会根据您的依赖关系自动配置您的应用程序。它不仅智能和有效，而且上下文智能，并记录您的需求。

让我们以数据库特性为例。如果您已经向 pom.xml 添加了一个需求，这个需求在某种程度上与数据库相关，那么 Spring boot 本身就意味着您想要使用数据库，因此它允许您的应用程序随时使用精确的数据库。

注释`@EnableAutoConfiguration`为 spring boot 启用自动配置，*框架使用它在其类路径中寻找* ***自动配置 bean****并自动应用它们*。它总是与`@Configuration`一起使用，如下所示:

```
@Configuration
@EnableAutoConfiguration
class BootAutoConfigExample{
…
}
```

如果您已经通过自己的 beans 提供了自己的配置，大多数自动配置会尊重您自己的配置，并自动退出。

## 设计独立的应用程序

*Spring boot 允许你设计* ***独立的、生产级质量的应用*** *你可以在任何网站上运行而不浪费时间。你可能认为运行一个 java 应用程序非常简单和容易。你所需要做的就是发出一个运行命令，然后一切都按照它应该的方式开始发生。但那只是你的假设(*反正是我的*😑).*

要运行 java 应用程序，需要执行以下步骤:

1.  打包您的应用程序
2.  选择要运行应用程序的 web 服务器类型，并下载应用程序
3.  配置 web 服务器
4.  组织部署流程

但是，如果您使用 Spring Boot 框架来运行您的应用程序，您只需遵循以下两个步骤:

1.  打包您的应用程序
2.  使用诸如*Java-jar my-application . jar*这样的命令运行您的应用程序

这就是你需要做的。 *Spring Boot 通过简单的* ***配置和部署一个嵌入式 web 服务器*** *就像*[*Apache Tomcat*](https://tomcat.apache.org/index.html)*到你的应用程序中，来处理你剩下的需求。*

## 固执己见的配置

正如在 [spring.io 文档](https://spring.io/projects/spring-boot)中提到的，

> “我们对 Spring 平台和第三方库有自己的看法，所以你可以毫不费力地开始。大多数 Spring Boot 应用需要最小的弹簧配置。”

我无法比这更简单地解释这个特性🤓。在开始构建或部署新的 Spring 应用程序之前，Spring Boot 坚持己见。当您使用 Java 时，您有足够的选项可以选择，从 web、日志、收集框架或您使用的构建工具开始。

开发人员喜欢只使用流行的库，而不是在 Java 中有这么多选择。Spring Boot 所做的只是以最标准的方式加载和配置它们。因此，开发人员不需要花费大量时间一遍又一遍地配置相同的东西。这样，他们就有更多的时间来编写代码和满足业务需求。

Spring Boot，*是类固醇上的春天*。这是一个很好的方法，可以很快开始使用几乎整个 Spring 堆栈。首先，它通过为您提供您最可能满意的智能默认配置，帮助您非常快速地设置一个完全工作的应用程序。

如果你像我一样，你会想要更多。因此，用参考文章来总结这一点，你可能想看看。

*   [**弹簧框架**](https://spring.io/projects/spring-framework)
*   [**创建自己的自动配置**](https://docs.spring.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-developing-auto-configuration.html)
*   [**Spring Boot GitHub 回购**](https://github.com/spring-projects/spring-boot/tree/main/spring-boot-project/spring-boot-starters)

如果你喜欢读这篇文章，你可能也会发现下面的文章值得你花时间去读。

[](https://medium.com/dev-genius/everything-a-developer-must-know-about-microservices-dae854782ab) [## 开发人员必须了解的关于微服务的一切

### 微服务是首选的应用平台，每个开发人员都必须了解它。

medium.com](https://medium.com/dev-genius/everything-a-developer-must-know-about-microservices-dae854782ab) [](https://medium.com/dev-genius/docker-the-most-loved-platform-every-developer-must-learn-6e5bb702c97b) [## docker——每个开发人员必须学习的最受欢迎的平台

### 实际上，根据 Stackoverflow 调查，它是第二大最受开发者喜爱的平台。

medium.com](https://medium.com/dev-genius/docker-the-most-loved-platform-every-developer-must-learn-6e5bb702c97b) [](https://medium.com/swlh/comprehensive-notes-for-java-8-features-every-developer-must-have-c08efc8ba39) [## 每个开发人员都必须具备的 Java 8 特性的综合说明

### Java SE 15 于 2020 年 9 月发布，附带一系列特性，但 Java 8 特性仍然是最…

medium.com](https://medium.com/swlh/comprehensive-notes-for-java-8-features-every-developer-must-have-c08efc8ba39) 

*如果你喜欢阅读有助于你更好地学习、生活和工作的故事，可以考虑* [*成为订阅者*](https://viveknaskar.medium.com/subscribe) *。成为会员后，你可以无限制地阅读 10000 篇故事、文章和作家。每月只要 5 美元。* [*如果你用我的链接*](https://viveknaskar.medium.com/membership) *注册，我会赚一点佣金，帮助我写更多的文章。*