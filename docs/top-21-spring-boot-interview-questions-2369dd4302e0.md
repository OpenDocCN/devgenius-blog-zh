# 21 大 Spring Boot 面试问题

> 原文：<https://blog.devgenius.io/top-21-spring-boot-interview-questions-2369dd4302e0?source=collection_archive---------3----------------------->

在过去的几个月里，我收到了一些关于 Spring Boot 面试问题的请求。在这篇文章中，我将回答 Spring Boot 面试中的 21 个问题。此外，我还将讨论一些与微服务架构相关的问题。

我将这些 Spring Boot 面试问题分为三类——新手、中级和有经验的开发人员。所以，如果你对 Spring Boot 没有任何经验，新手问题会帮助你。

# Spring Boot 面向开发新手的面试问题

# 1.什么是 Spring Boot，它在 Web 应用程序开发中有什么用处？

Spring Boot 是一个建立在 Spring 框架之上的框架，用于快速应用程序开发。同样，Ruby on Rails，Django with Python web frameworks 也很有名。特别是，Spring 为 Java 带来了一个非常需要的 web 框架来构建 web 应用程序。嵌入式应用服务器和自动配置等重要特性支持快速开发和部署。

*   Spring Boot 框架允许轻松导入依赖关系。
*   它还以其丰富的特性简化了在应用程序中使用 Spring Security 的过程。
*   [致动器](https://betterjavacode.com/java/spring-boot-actuator) —可以实现生产就绪特性来监控应用程序。

# 2.Spring Boot 和春天有什么不同？

Spring 框架包含了可以在 Spring Boot 框架中使用的多个特性。另一方面，Spring Boot 框架用于应用程序开发。

同样，Spring framework 提供了数据绑定、依赖注入、面向方面编程、数据访问、安全性等特性。

简而言之，Spring Boot 是 Spring 框架的模块之一。Spring Boot 是建立在 Spring 框架之上的。Spring Boot 是一个基于微服务的框架来构建应用程序。Spring Boot 的自动配置特性有助于减少代码行。

# 3.如何用 maven 或 gradle 设置 Spring Boot 应用程序？

要设置一个 spring boot 应用程序，可以使用 maven 或 gradle。两者都用于在应用程序中提取所需的依赖关系。具体来说，对于 spring boot 应用程序，可以使用`spring-boot-starter-parent`依赖关系。

在 maven 中，使用`pom.xml`文件，在 gradle 中，使用`build.gradle`进行依赖声明

Maven:

```
<groupId>org.springframework.boot</groupId> <artifactId>spring-boot</artifactId> <version>2.4.1</version>
```

格拉德:

```
compile group: 'org.springframework.boot', name: 'spring-boot', version: '2.4.1'
```

# 4.如何更改 Spring Boot 应用程序的默认端口？

有三种方法可以更改 Spring Boot 应用程序的默认端口。因此，最直接的方法是使用`application.properties`或`application.yml`文件。

*   `server.port`在`application.properties`中使用该属性。
*   您还可以在主`@SpringBootApplication`类中以编程方式更改端口。
*   当应用程序作为 jar 文件启动时，您可以使用您选择的端口。`java -jar -Dserver.port=8980 myapplication.jar`

# 5.你会如何设置一个应用程序与 HTTPS 港与 Spring Boot？

默认情况下，Spring Boot 应用程序在 HTTP 端口上运行。要使您的应用程序运行 HTTPS 端口，有两种方法。

*   可以使用`server.port`、`server.ssl.key-store-password`、`server.ssl.key-store`、`server.ssl.key-store-type`和`server.ssl.key-alias`。
*   使用 HTTPS 端口的另一种方式是编程。

# 6.Spring Boot 支持哪些嵌入式服务器？

Spring Boot 应用程序支持 Tomcat、Jetty 和 Undertow 服务器。默认情况下，Spring Boot 的 web starter 支持 Tomcat web 服务器。

要更改默认的 web 服务器，必须排除`spring-boot-starter-tomcat`并包含相应的服务器启动器，如 jetty 或 undertow。

```
<dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-web</artifactId> <exclusions> <exclusion> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-tomcat</artifactId> </exclusion> </exclusions> </dependency> <dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-jetty</artifactId> </dependency>
```

# 7.Spring Boot 的优势是什么？

以下是 Spring Boot 的主要优势

*   允许创建独立的 Spring 应用程序
*   人们可以很容易地使用 tomcat、jetty 或 undertow 服务器
*   迅速发展
*   还允许轻松使用基于微服务的架构

# 中级开发人员的 Spring Boot 面试问题

到目前为止，我已经介绍了一些基本的面试问题。如果您是一名有几年 Java 或 Spring Boot 开发经验的开发人员，下一组问题将帮助您在哪些方面有所改进。

# 8.如何将 Spring Boot Web 应用程序部署为 jar 和 war 文件？

通常，我们将 web 应用程序部署为 web 归档文件(WAR)。我们在外部 web 服务器上部署 war 文件。

显然，如果使用 maven 构建应用程序，Spring 会提供一个插件`spring-boot-maven-plugin`。然后，您可以将 web 应用程序打包成一个可执行的 jar 文件。如果使用 gradle，`gradle build`会默认创建一个可执行的 jar 文件。

对于 maven 来说，

`<packaging>jar</packaging>`将构建一个 jar 文件。

`<packaging>war</packaging>`将建立一个 war 文件。如果没有包含打包元素，maven 将默认 jar 文件。您还需要保持容器依赖关系，如下所示:

```
<dependency> 
<groupId>org.springframework.boot</groupId> 
<artifactId>spring-boot-starter-tomcat</artifactId> <scope>provided</scope> 
</dependency>
```

对于 gradle，您需要将嵌入式容器的依赖项标记为属于 war 插件的`providedRuntime`配置。

# 9.为什么我们在主 Spring Boot 应用程序类中使用注释`@SpringBootApplication`？

对于使用自动配置、组件扫描和额外配置的应用程序，可以包含一个注释`@SpringBootApplication`。该注释包括三个注释`@EnableAutoConfiguration`、`@ComponentScan`和`@Configuration`。

*   `@EnableAutoConfiguration` -启用 Spring Boot 的[自动配置](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/using-boot-auto-configuration.html)
*   `@ComponentScan` -这允许扫描应用程序所在的包。
*   `@Configuration` -允许在上下文中注册额外的 beans 或导入额外的配置类。

# 10.`@RestController`和`@RequestMapping`标注是做什么用的？

`@RestController` -该注释增加了注释`@Controller`和`@ResponseBody`。该注释标记了一个将处理传入请求的类。通常，该注释用于创建 RESTful APIs。

`@RequestMapping` -该注释在处理传入请求的类中提供路由信息。这个注释将传入的 HTTP 请求映射到相应的方法。

# 11.为什么我们需要弹簧剖面？

当构建企业应用程序时，我们使用不同的环境，如开发、测试、QA 和生产。Spring Boot 允许为每种环境配置不同的应用程序属性。

为了帮助分离每个环境的配置，可以根据环境来命名属性文件，如`application-dev.properties`、`application-test.properties`和`application-prod.properties`。

# 12.如何禁用特定的自动配置？

要禁用任何特定的自动配置，可以使用`@EnableAutoConfiguration`注释的`exclude`属性。

```
@EnableAutoConfiguration(exclude = DataSourceAutoConfiguration.class) public class SampleApplication { }
```

# 13.百里香叶是什么，怎么用？

Thymeleaf 是一个 web 应用程序的服务器端模板引擎。它的主要目的是为 web 应用程序带来自然的模板。

具体来说，必须在应用程序中包含依赖关系`spring-boot-starter-thymeleaf`才能使用百里香叶。

# 14.有哪些不同的 Spring Boot 启动器，你可以将它们用于什么目的？

强调一下，Spring Boot 的一个主要优势是能够为您的 Spring Boot 应用程序使用各种依赖关系。所有启动器 Spring Boot 依赖项都在`org.springframework.boot`组下。这些依赖关系从`spring-boot-starter-`开始。

有 50 多个初学者依赖项，但这里列出了大多数开发人员将在他们的应用程序中使用的依赖项:

*   `spring-boot-starter`——核心启动器。这包括自动配置、日志记录和 YAML。
*   `spring-boot-starter-data-jpa` -这包括带 hibernate 的 Spring 数据 JPA。
*   `spring-boot-starter-security` - Spring 安全模型为您的 web 应用程序提供了安全性。默认情况下，它将添加基本的基于表单的身份验证。
*   `spring-boot-starter-web` -这包括一个 web 模块，有助于使用 Spring MVC 创建 web、RESTful 应用程序。
*   `spring-boot-starter-thymeleaf` -导入这个模块允许开发者在应用中使用百里香模板引擎。
*   `spring-boot-starter-jdbc` -这个模块有助于连接到应用程序的数据库。
*   `spring-boot-starter-tomcat` -通常，这是 spring boot 父模块的一部分。这允许开发人员使用嵌入式 tomcat servlet 容器。

# Spring Boot 为有经验的开发人员准备的面试问题

下一组问题是为有经验的开发人员准备的。尽管有区别，初学者和中级开发人员都应该了解这些问题。

# 15.如何使用 JPA 将 Spring Boot 应用程序连接到数据库？

Spring Boot 提供了将 Spring 应用程序连接到关系数据库的依赖关系。

另一方面，Spring Data JPA 处理基于 JDBC 的数据库访问和对象关系建模的复杂性。因此，它改进了数据访问层处理的实现。从开发人员的角度来看，它减少了对关系数据库查询的依赖。

JPA 允许将应用程序类映射到数据库表。

# 16.你为什么使用 Spring Boot 执行器？

[Spring Boot 致动器](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html)为应用带来生产就绪特性。因此，可以很容易地监控应用程序。

它还为应用程序提供审计、健康检查和不同的指标。因此，您需要包括如下相关性:

```
<dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-actuator</artifactId> <scope>provided</scope> </dependency>
```

以下是这种依赖关系为应用程序提供的一些端点:

*   `env` -显示所有环境属性的端点
*   `health` -显示应用程序健康状况的端点
*   `metrics` -此端点显示应用程序的各种指标
*   `loggers` -显示应用程序中记录器的配置
*   `httptrace` -显示 HTTP 跟踪信息的端点

# 17.Spring Boot 开发工具是用来做什么的？

Spring Boot 开发工具或开发工具是一套工具，使开发过程更容易。要包含 DevTools 支持，必须在项目中添加依赖项`spring-boot-devtools`。

Spring Boot DevTools 有助于禁用许多 Spring Boot 依赖项使用的缓存。这在开发过程中很有用，因为开发人员希望立即看到变更。

此外，使用这个库允许应用程序在类路径上的文件发生变化时重新启动。

此外，该库支持 web 组的调试日志记录。

# 18.使用 Spring Data JPA 有什么好处？

Spring Data Jpa 提供 JpaTemplate 来集成 Spring 应用程序和 Jpa。您还可以选择 JPA 规范的具体实现。默认会是`Hibernate`。

Spring Data JPA 的优势

*   减少样板代码
*   生成的查询有助于减少开发人员对数据库查询的依赖
*   存储库模式允许开发人员轻松处理持久性。

# 19.解释 Spring Boot 支持宽松的约束。

通常，属性的键需要与 Spring Boot 应用程序中的属性名完全匹配。Spring Boot 支持宽松绑定，这意味着属性的键不必与属性名完全匹配。

此外，环境属性可以在任何情况下编写。示例—如果您的 bean 类中有一个带有注释`@ConfigurationProperties`的属性`propertyDB`，您可以将该属性绑定到这些环境属性中的任何一个- `PROPERTYDB`、`propertydb`或`property_db`。

# 20.如果您想在应用程序中使用 Spring Security，那么如何使用它，以及它如何使应用程序安全？

目前，为了保护您的 Spring Boot 应用程序，您可以在您的项目中包含`spring-boot-starter-security`依赖项。默认情况下，它会为您的应用程序添加一些安全措施。这些默认措施包括向应用程序添加基本的基于表单的身份验证，或者保护 REST APIs。

随后，为了利用 Spring 安全性，可以扩展`WebSecurityConfigurerAdapter`类来添加定制的安全措施。在同一个类中，您将使用`@EnableWebSecurity`注释。该注释允许 Spring 查找配置并将该类应用于全局 WebSecurity。

# 21.你用注释@ControllerAdvice 做什么？

`@ControllerAdvice`注释有助于管理 Spring Boot 应用程序中的例外。简而言之，它是用`@RequestMapping`注释的方法抛出的异常的拦截器。

`ResponseEntityExceptionHandler`是提供集中式异常处理的基类。

到目前为止，我已经涵盖了 Spring Boot 面试问题。我还是会涵盖几个可以帮助开发者的微服务面试问题。

# 微服务面试问题

# 1.什么是微服务？

微服务是一种架构风格。它将应用程序构建为服务的集合，这些服务

*   高度可维护
*   松散耦合
*   易于部署
*   由一个小团队拥有

# 2.构建和部署微服务常用的工具有哪些？

这取决于技术栈。Docker 仍然是一个不考虑技术栈的通用工具。因此，docker 允许为微服务创建一个容器。然后，这个容器可以部署在云环境中。

Wiremock 是另一个针对微服务的灵活 API 模拟工具。

Hysterix 是一个断路器，有助于应用程序的延迟和容错。

# 3.Spring 对微服务开发有什么帮助？

最重要的是，Spring 有助于快速开发。 [Spring Boot](https://betterjavacode.com/books) 帮助构建 REST APIs 和 web 应用。Spring Cloud 有助于与外部系统集成。

# 4.微服务面临哪些挑战？

微服务互相依赖。因此，需要保护这些服务之间的通信。由于微服务是一个分布式系统，它可以成为一个复杂的模型。可能会有操作开销。更多的服务意味着更多的资源。

# 结论

在这篇文章中，我涵盖了你在面试中可能会遇到的关于 Spring Boot 的大多数问题。如果你喜欢这篇文章，请在这里订阅我的博客。

*原载于 2021 年 1 月 9 日 https://betterjavacode.com*[](https://betterjavacode.com/spring-boot/top-21-spring-boot-interview-questions)**。**