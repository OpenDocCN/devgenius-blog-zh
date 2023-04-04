# liqui base—Spring Boot 的处理数据库

> 原文：<https://blog.devgenius.io/liquibase-handling-database-in-spring-boot-dbdf237d2b3b?source=collection_archive---------2----------------------->

![](img/5bbbf80902eb930cb1a6de2cecc6f495.png)

照片由[卡斯帕·卡米尔·鲁宾](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果您正在用 Spring Boot 构建一个应用程序，随着时间的推移，处理数据库更改将成为一场噩梦。添加的更改越多，需要维护的数据库更改就越多。Liquibase 是最好的解决方案。在本帖中，我们将展示如何使用 liquibase 处理数据库更改。

# 什么是 Liquibase？

Liquibase 是一个用于跟踪、管理和应用数据库变化的开源库。Liquibase 通过 XML 配置跟踪数据库的变化，开发人员通常会在 XML 配置中添加变更集。

每个变更集都有一个 id 和作者属性。Liquibase 使用 changelog 来跟踪数据库的变化。您添加的每个变更集都会被添加到变更日志中。Changelog 是您对数据库所做的所有更改的分类帐。

# Liquibase 是如何工作的？

为了跟踪数据库的变化，您将编写一个独立于平台的 XML 文件。这个 XML 文件将在命令行上用于翻译成数据库引擎的脚本。

我们还可以使用 maven 或 Gradle 插件在构建配置中包含数据库更改。

Liquibase 使用自己的表来跟踪变化。出于一致性目的，这些表将成为您正在构建的模式的一部分。它记录每个变更集的哈希。

# 如何写变更集？

之前，我提到过您可以使用 XML 编写变更集。但是 liquibase 也提供了对 JSON 或 YAML 的支持。

作为这篇文章的一部分，我将展示如何添加变更集并为数据库生成脚本。

在文件夹`src\main\resources\db`下为我们的数据库创建一个 XML changelog 文件`db.changelog-master.xml`。通常，如果您从项目一开始就开始使用 liquibase，您将创建一个初始的 changelog 文件，该文件将生成初始脚本。您可以通过变更集跟踪此后的每一个变更。

没有任何变更集的文件将如下所示:

现在我可以用两种方式处理这个主文件。对于每个变更集，我可以创建一个单独的文件并将该文件包含在主文件中，也可以将每个变更集添加到同一个主文件中。

每个变更集都需要一个作者和唯一的 id。

现在，我们将把变更集添加到这个 changelog 文件中，它将如下所示:

现在我们准备在我们的 Spring Boot 项目中创建 liquibase Bean。我们必须在我们的`application.properties`文件中添加以下属性。

`spring.liquibase.changeLog=classpath:/db/db.changelog-master.xml`。

另外，不要忘记在`application.properties`文件中添加数据库属性。

在运行我们的 Spring Boot 项目之前，在我们的 gradle 项目中添加 liquibase 依赖项。

`compile('org.liquibase:liquibase-core:4.0.0')`。

现在，如果我们运行我们的 Spring Boot 项目，我们将在日志消息中看到创建的数据库表，如下所示:

作为执行的一部分，liquibase 还创建了表`databasechangelog`和`databasechangeloglock`。Liquibase 使用这些表来跟踪数据库的变化。如果您在 changelog 文件中添加另一个变更集，liquibase 将根据以前的更改识别该变更集，并在您下次运行应用程序时执行适当的操作。

# 结论

在本文中，我展示了如何在 Spring Boot 项目中使用 liquibase 处理数据库更改。

我在这篇文章中没有讨论的一件事是另一个数据库迁移工具 Flyway。 [Flyway](https://flywaydb.org/) 也是一款开源的数据库迁移工具。

*原载于 2020 年 7 月 26 日 https://betterjavacode.com**[*。*](https://betterjavacode.com/programming/liquibase-handling-database-in-spring-boot)*