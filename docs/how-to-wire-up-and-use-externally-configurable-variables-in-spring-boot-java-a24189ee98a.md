# 如何在 Spring Boot + Java 中连接和使用外部可配置变量

> 原文：<https://blog.devgenius.io/how-to-wire-up-and-use-externally-configurable-variables-in-spring-boot-java-a24189ee98a?source=collection_archive---------3----------------------->

![](img/710969a26359222686bae827fd09d331.png)

Aniket Bhattacharya[原图](https://unsplash.com/@aniket940518)；标志由[枢转](https://spring.io/projects/spring-framework)；作者:Tremaine Eto

Spring Boot 之所以令人敬畏有很多原因，其中一个原因是它易于设置外部配置，这样您就可以在不同的环境中运行相同的应用程序代码，或者随时更新变量，而不必进去更改代码本身。

本文将介绍如何使用注解`@Component`和`@ConfigurationProperties`在 Spring Boot 的 Java 应用程序中实现这一点。

# 配置类声明

首先，您必须创建一个 Java 类，它将通过`@Component`注释成为一个 Spring Boot bean。

在其中，变量将是您希望外部可配置的变量。如果这些是`String`值——比如用户名或电子邮件地址——那么就这样声明它们；如果这些是`int`值——比如端口号或线程数——那么相应地声明它们。

在这个例子中，我们有一个`EmailConfig`类，用于在我们的代码中发送电子邮件的可配置变量。我们允许灵活地:

*   用`enabled` `Boolean`变量打开和关闭功能；
*   用`senderEmail` `String`值编辑发件人邮件；
*   用`String`值的`recipientEmails`列表编辑收件人电子邮件列表；
*   用`subject`变量编辑电子邮件的主题

# 应用程序.属性

我们可以在我们的`application.properties`(有时是`application.yml`)文件中为这些定义默认值，如下所示:

这里重要的是，这些都有前缀`email`——如果您向上滚动到我们定义的 bean，您会注意到有一个专门寻找这个前缀的`@ConfigurationProperties`注释。

Spring Boot 会施展自己的魔法，然后根据变量的名称将它们匹配起来；注意大小写，因为 Spring Boot 会根据其[宽松的绑定规则](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config-relaxed-binding)尝试匹配。

在这个例子中，我遵守了 camelCase 约定，但是您也可以将`email.senderEmail`定义为`email.sender-email`(使用连字符)或`email.sender_email`(使用下划线)，多亏了 Spring Boot，这样也没问题。

# 在代码中使用配置

现在我们有了 bean `EmailConfig`，我们可以在 Java 应用程序中使用它，如下所示:

请注意，为了简单起见，这只是一个示例，我们通过将值打印到控制台来测试配置设置是否有效。在您的实际应用程序中，您将能够像使用硬编码变量一样使用这些变量来执行发送电子邮件的逻辑。

当谈到减少手动步骤和不必要的代码更改和部署时，外部配置绝对是至关重要的，这是 Spring Boot 的一个关键特性，需要加以利用。

我希望你能更好地了解如何用这种特定的方法来做这件事，对于进一步的阅读，我强烈建议阅读关于这个主题的 [Spring Boot 文档](https://docs.spring.io/spring-boot/docs/1.5.6.RELEASE/reference/html/boot-features-external-config.html)。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)