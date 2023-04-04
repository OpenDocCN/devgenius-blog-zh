# 代码气味 106 —依赖于生产的代码

> 原文：<https://blog.devgenius.io/code-smell-106-production-dependent-code-f74dbbfde0cf?source=collection_archive---------6----------------------->

## 不要为生产环境添加 IFs 检查。

![](img/327e1170a2d8a876b7ee6e29b84c6afc.png)

> TL；DR:避免添加与生产相关的条件句

# 问题

*   失败快速原则违反
*   缺乏可测试性

# 解决方法

1.如果完全有必要，模拟环境并测试所有的环境。

# 语境

有时候，我们需要在开发和生产中创造不同的行为。

例如密码的强度。

在这种情况下，我们需要用优势策略配置环境，并测试策略而不是环境本身。

# 示例代码

## 错误的

```
def send_welcome_email(email_address, environment):
 if ENVIRONMENT_NAME == “production”:
 print(f”Sending welcome email to {email_address} from Bob Builder <[bob@builder.com](mailto:bob@builder.com)>”)
 else:
 print(“Emails are sent only on production”)

send_welcome_email(“[john@doe.com](mailto:john@doe.com)”, “development”)
# Emails are sent only on productionsend_welcome_email(“[john@doe.com](mailto:john@doe.com)”, “production”)
# Sending welcome email to [john@doe.com](mailto:john@doe.com) from Bob Builder <[bob@builder.com](mailto:bob@builder.com)>
```

## 对吧

```
class ProductionEnvironment:
 FROM_EMAIL = “Bob Builder <[bob@builder.com](mailto:bob@builder.com)>”class DevelopmentEnvironment:
 FROM_EMAIL = “Bob Builder Development <[bob@builder.com](mailto:bob@builder.com)>”

#We can unit test environments
#and even implement different sending mechanismsdef send_welcome_email(email_address, environment):
 print(f”Sending welcome email to {email_address} from {environment.FROM_EMAIL}”)
 #We can delegate into a fake sender (and possible logger)
 #and unit test itsend_welcome_email(“[john@doe.com](mailto:john@doe.com)”, DevelopmentEnvironment())
# Sending welcome email to [john@doe.com](mailto:john@doe.com) from Bob Builder Development <[bob@builder.com](mailto:bob@builder.com)>send_welcome_email(“[john@doe.com](mailto:john@doe.com)”, ProductionEnvironment())
# Sending welcome email to [john@doe.com](mailto:john@doe.com) from Bob Builder <[bob@builder.com](mailto:bob@builder.com)>
```

# 侦查

[X]手册

这是一种设计气味。

我们需要创建空的开发/生产配置，并委托可定制的多态对象。

# 标签

*   连接

# 结论

避免添加不可测试的条件句。

创建委派业务规则的配置。

使用抽象、协议和接口，避免硬性层次结构。

# 关系

[](/code-smell-56-preprocessors-89d8beb6655b) [## 代码气味 56 —预处理器

### 我们希望我们的代码在不同的环境和操作系统下表现不同，所以在编译时做出决定…

blog.devgenius.io](/code-smell-56-preprocessors-89d8beb6655b) 

# 更多信息

[](/how-to-get-rid-of-annoying-ifs-forever-317033474484) [## 如何永远摆脱烦人的 if

### 为什么我们学习编程的第一条指令应该是最后使用的。

blog.devgenius.io](/how-to-get-rid-of-annoying-ifs-forever-317033474484) 

这条推文的灵感来自[简·贾科姆利](https://medium.com/u/b6b5a296f32f?source=post_page-----f74dbbfde0cf--------------------------------)

> 复杂是技术不成熟的标志。无论是 ATM 还是爱国者导弹，简单易用都是设计良好的产品的真正标志。

*林志玲*

[](/software-engineering-great-quotes-3af63cea6782) [## 软件工程名言

### 有时一个简短的想法可以带来惊人的想法。

blog.devgenius.io](/software-engineering-great-quotes-3af63cea6782) 

本文是 CodeSmell 系列的一部分。

[](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到你的代码中有问题的部分

### 代码很难闻。让我们看看如何改变香味。

blog.devgenius.io](/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)