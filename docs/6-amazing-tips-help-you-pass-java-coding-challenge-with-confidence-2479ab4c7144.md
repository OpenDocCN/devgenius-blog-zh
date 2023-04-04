# 6 个惊人的技巧帮助你自信地通过 Java 编码挑战

> 原文：<https://blog.devgenius.io/6-amazing-tips-help-you-pass-java-coding-challenge-with-confidence-2479ab4c7144?source=collection_archive---------2----------------------->

## 除了 Java 编程和问题解决技巧之外的基本技术

![](img/094ed0649829a52caf77b3e264248ffd.png)

[Icons8 团队](https://unsplash.com/@icons8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/photos/MYbhN8KaaEc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

我是一名面试官，负责在工作面试的编码挑战环节中验证候选人的动手编码技能。遗憾的是，10 个考生中有 7 个没有通过考试。事实上，编码练习并不涉及复杂的算法和数据结构，而只是一个用排序和过滤逻辑构建 REST APIs 的练习。

许多公司都有自己的一套评估标准，当涉及到编码挑战时，有些公司可能专注于问题的解决，而有些公司则强调数据结构的有效使用。除了解决问题的技巧之外，考生还应该展示以下 Java 编程的技能和知识:

1.  Java 编码
2.  Spring 框架知识
3.  结构良好的代码设计
4.  单元测试的实现

在本文中，我将与您分享一系列技巧，帮助您为 Java 后端开发人员这一角色的编码挑战做好准备。

# 技巧#1 —准备您的开发环境

由于编码挑战会议通常是远程在线进行的，当您在自己的 PC 上处理解决方案时，您应该共享您的屏幕。在面试之前，在你的电脑上做好一切准备是至关重要的。然而，我偶尔观察到一些候选人没有准备就来了，他们只是在面试期间花时间在 IDE 和 JDK 安装上！

大多数编码挑战会议的持续时间约为 1 小时。时间非常紧迫，因为理解问题、设计解决方案、实现代码逻辑和测试都需要时间。浪费时间准备工作空间意味着你可能无法在 1 小时内完成练习。

如果编码挑战将在您的 PC 上通过共享屏幕完成，那么下面是一个建议项目列表，这些项目将在面试前设置好:

*   安装最新的 JDK
*   安装 IDE——强烈推荐 Intellij，因为它提供了自动完成功能，大大加快了编码速度
*   使用 Spring Initializr(【https://start.spring.io/】T4)建立一个示例 maven 项目，如果需要的话，选择常用的依赖项，比如 Spring Web
*   确保示例项目可以在您的本地工作区上成功编译

示例项目启动并运行后，您就可以在面试会话期间根据示例项目开始编写练习代码了。

# 技巧 2——使用 Lombok 节省样板代码的时间

[Lombok](https://projectlombok.org/) 是一个你不应该忽视的热门图书馆。虽然有些人反对使用 Lombok，因为它在编译时神秘地修改了字节码，但不可否认的是，它加快了开发速度，因为它消除了编写乏味和重复的样板代码的需要。

如果你从未听说过龙目岛，这里有一个简单的例子。通常，Java 类有 setters 和 getters 来访问私有属性。此外，还有一个默认的构造函数:

通过使用 Lombok 注释，由于库自动生成样板代码，代码行大大减少了

使用 Lombok 可以让您专注于逻辑实现，而不是在编码期间花时间在样板代码上。同时，它也展示了你对这个流行图书馆的了解。

如果你想更多地了解龙目岛，请参考这篇文章:

[](/how-to-magically-speed-up-java-coding-76fa1a68e0f4) [## 如何神奇地加速 Java 编码

### 使用 Lombok 少写多做

blog.devgenius.io](/how-to-magically-speed-up-java-coding-76fa1a68e0f4) 

# 技巧 3——使用组织良好的代码结构

控制器-服务-存储库模式是基于 Spring 框架的应用程序开发的常见模式。在编码过程中构建解决方案时，坚持这种结构，您将拥有一个组织良好的代码框架。这种模式的概念是关注点分离，即将整个应用程序的不同方面解耦:

*   **控制器** —暴露 web / API 端点。它是 HTTP 协议和内部函数调用之间的集成层，因此它没有任何业务逻辑
*   **服务**——服务组件持有核心业务逻辑
*   **存储库** —集成层到持久存储

本文向您展示了如何在一个定义良好的结构中构建 Spring Boot 应用程序:

[](/spring-a-faster-way-to-build-production-ready-api-in-a-well-defined-structure-5b1730fa81dd) [## spring——一种在定义良好的结构中构建生产就绪 API 的更快方法

### 以可维护且一致的结构构建您的应用程序

blog.devgenius.io](/spring-a-faster-way-to-build-production-ready-api-in-a-well-defined-structure-5b1730fa81dd) 

# 技巧 4 —熟悉常用的 Spring 注释

嗯，期望候选人记住所有 Spring 注释是不公平的。但是，如果你想给人一种你是一个有经验的动手开发人员的印象，那么你应该能够使用那些常用的注释，而不用在互联网上搜索。

以下是一些常用的注释:

```
@RestController
@RequestMapping
@PathVariable
@ReqeustParam
@Valid
@Autowired
@Service
@Repository
```

# 技巧 5 —函数式编程更好

与使用传统的 for / while 循环不同，使用函数式编程简化了代码，使其易于阅读。如果你能展示这种现代编码风格，你将获得加分。

如果您想快速掌握函数式编程的技能，请参考下面的文章和一组练习:

[](/15-practical-exercises-help-you-master-java-stream-api-3f9c86b1cf82) [## 15 个实用练习帮助你掌握 Java Stream API

### 使用强大的 Java 流 API 简化您的代码逻辑

blog.devgenius.io](/15-practical-exercises-help-you-master-java-stream-api-3f9c86b1cf82) 

# 技巧 6——通过单元测试来验证你的代码，而不是启动整个系统

许多候选人一旦完成了 Rest 控制器的简单框架，就启动 Spring Boot 应用程序，然后使用 web 浏览器或邮递员来验证端点。这种实践是一种老派的验证开发的方式，而且这种手动过程是缓慢和低效的。

如今，现代科技公司的软件工程师不仅要构建系统逻辑，还要构建自动化测试，作为业务功能交付的一部分。

有经验的软件工程师只有在组件的单元测试和集成测试全部通过后，才能启动整个应用程序进行手工测试。所有组件都正常工作，您确信整个系统工作正常，没有任何问题。

因此，最好构建单元测试来验证您的代码。运行单元测试和故障排除比启动整个系统要快得多。首先，您可以首先编写一个简单的单元测试来验证 API 端点的响应状态。然后，随着事情进展顺利，添加更多的测试，如错误场景和边缘案例。

寻找自动化测试的编码练习？本文为您提供了一组如何为 Spring Boot 应用程序构建自动化测试的实践练习。

[](/7-practical-exercises-help-you-acquire-hands-on-test-development-of-spring-boot-applications-dd0952a8af6) [## 7 个实践练习帮助您获得 Spring Boot 应用程序的实际测试开发

### 关于自动化测试开发你需要知道的一切——mock ITO、AssertJ、WireMock、Testcontainers

blog.devgenius.io](/7-practical-exercises-help-you-acquire-hands-on-test-development-of-spring-boot-applications-dd0952a8af6) 

# 最后的想法

编码挑战赛是整个招聘过程中最紧张的活动之一，它涵盖了广泛的知识领域，如问题解决、算法、数据结构、编码等。

即使您在 Java 编程方面很强，具有解决问题的能力，但是如果没有适当的准备和动手编程实践的演示，您也无法轻松通过测试。要想成功，你需要确保你的本地开发环境已经准备好，避免在样板代码上浪费时间，以一种有组织的方式构建解决方案，熟悉 Spring 框架，展示现代编码风格，并实现自动化测试而不是手动试错。