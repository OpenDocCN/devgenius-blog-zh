# 第 2 部分—如何:在 Spring Boot 配置属性

> 原文：<https://blog.devgenius.io/part-2-how-to-configure-properties-in-spring-boot-783c54a39304?source=collection_archive---------6----------------------->

我确信所有的开发人员都喜欢他们正在处理的代码整洁干净。编写干净的代码是一项令人愉快的活动，它让我们都为自己的技能感到自豪；然而，查看干净的代码更加令人满意。

Spring Boot 项目中的属性文件在一个位置包含所有具有不同用途的属性，管理久而久之会变得非常困难，并且代码库会增长。

在本教程中，我将教你一种最基本的方法，根据属性的含义和用途对属性进行分类。这将使开发过程以及未来的功能增强更加容易。

## 创建属性文件

向 Spring Boot 添加一个新的属性文件就像在类路径的 resources 文件夹中添加一个新文件一样简单。

假设您想要在`***project/src/main/resources/properties***`中创建一个名为`***mail.properties***`的新文件。将以下配置添加到文件中( ***插入有效信息，而不是粗体短语*** ):

```
mail.host=***mail.host.domain***
mail.port=***port***
mail.transport.protocol=smtp
mail.properties.smtp-starttls-enable=true
mail.username=***username@domain***
mail.password=***password***
mail.properties.mail.smtp.auth=true
mail.sender.address=***mail-sender-address@domain***
```

在这个文件中，我们列出了在应用程序中设置电子邮件服务可能需要的所有内容。请记住，我们一直使用同一个关键字来定义属性——“邮件”稍后，我将解释原因。

## 在 Spring Boot 应用程序中注册属性文件

一旦准备好属性文件，就可以创建一个表示这些属性的类。在`***project-name/src/main***`或其任何子文件夹中创建一个新的 Java 类`***MailProperties***`，并将以下代码放入其中:

```
@Data
@Lazy
@Configuration
@PropertySource("classpath:/properties/mail.properties")
@ConfigurationProperties(prefix = "mail")
public class MailProperties {

    @Value("${mail.host}")
    private String host;

    @Value("${mail.port}")
    private Integer port;

    @Value("${mail.transport.protocol}")
    private String transportProtocol;

    @Value("${mail.properties.smtp-starttls-enable}")
    private String enableSmtpStartTls;

    @Value("${mail.username}")
    private String username;

    @Value("${mail.password}")
    private String password;

    @Value("${mail.properties.mail.smtp.auth}")
    private String smtpAuth;

    @Value("${mail.sender.address}")
    private String senderAddress;
}
```

为了让我们的生活更轻松，我们加入了一些注释:

`***@Data***`是一个方便的 Lombok 注释，包括`@Getter`、`@Setter`、`@ToString`、`@EqualsAndHashCode`和`@RequiredArgsConstructor`注释，并生成通常与 POJOs 相关的所有样板代码。您可以将 ***Lombok*** 添加到您的依赖项中并使用它，或者您可以包含构造函数和其他结构。

由于默认情况下，Spring Boot 会在应用程序上下文开始时急切地创建所有 bean，`***@Lazy***`确保只有在我们请求时才会创建 bean。当`@Lazy`注释被应用到`@Configuration`类时，这意味着所有方法都应该被延迟加载。

当与`***@PropertySource***`注释结合使用时，`***@Configuration***`注释提供了一种简单的方法来将属性源添加到环境中，处理类，并创建合适的 beans。

您可能还记得，我们的属性被分组在一起，并以“邮件”开始。在我们使用`***@ConfigurationProperties(prefix = “mail”)***`输入属性前缀之后，Spring Boot 应用它的配置技术，自动在属性名和它们各自的字段之间进行映射。

但是，您可能希望为本地字段指定与其对应的属性不同的名称。您可以通过用`***@Value(“$mail.host”)***`注释字段来做到这一点，它指定了属性的确切名称。

## 使用属性配置类

当你完成这个配置类时，你可以 ***将它*** 注入到你的应用中的任何地方，并根据需要使用它:

```
@Service
@Log4j2
@AllArgsConstructor
public class EmailService {
    private final MailProperties mailProperties;

    public void testProperties() {
        String host = mailProperties.getHost();
        String password = mailProperties.getPassword();
        String username = mailProperties.getUsername();
        // ... *and so on* }
}
```

在这篇短文中，我们以邮件属性为例，探讨了如何在 Spring Boot 中配置属性。

当然，这不是关于在 Spring Boot 应用程序中指定属性的最全面的指南；还有许多更高级的选项。然而，本教程应该给你一个坚实的概念，从哪里开始，它应该足够小规模的应用程序。

在下一篇教程中，我将向您展示如何为 Spring Boot 项目配置邮件发送者服务。

***敬请关注，在评论区随意推荐你希望在本系列后续文章中讨论的话题！***