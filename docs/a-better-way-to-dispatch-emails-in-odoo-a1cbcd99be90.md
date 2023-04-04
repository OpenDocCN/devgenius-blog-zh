# 在 Odoo 中发送电子邮件的更好方式

> 原文：<https://blog.devgenius.io/a-better-way-to-dispatch-emails-in-odoo-a1cbcd99be90?source=collection_archive---------5----------------------->

## 理想的实现不知道电子邮件最终是如何发送出去的

![](img/47c3df97d69682b8676541b47e053f33.png)

由作者设计

Odoo 中的许多特性可以用多种方式实现。

在 Odoo 中发送电子邮件也是如此。

在这篇文章中，我将分享在 Odoo 中调度邮件的两个实现。

第一个糟糕的实现在一些 Odoo 开发者中很常见，大多数是新手和业余爱好者。第二个理想的实现是首选，因为它更简单、简洁，并且利用了[委托模式](https://en.wikipedia.org/wiki/Delegation_pattern)。

# 在我们继续之前，让我们定义一个基本要求

作为 Odoo 开发人员，您的项目经理分配给您一项任务，为房地产经纪人模块实现电子邮件通知。该模块允许房地产开发商(或经理)向潜在买家列出他们的房产。

你的任务是实现一个功能，支持电子邮件通知房地产经纪人每一次买方表现出兴趣的财产。

对所列物业感兴趣的买家将点击物业表格上带有“我感兴趣”标签的按钮，向物业经理发送电子邮件。

让我们看看使用 Odoo 实现这一需求的两种方法

# 执行不力

奇怪的是，我们仍然使用电子邮件通知的常见实现，我们将采取以下步骤。

1.  使用 [QWeb 模板定义一个电子邮件模板。](https://www.odoo.com/documentation/16.0/developer/reference/frontend/qweb.html)

当潜在客户对他们的任何房地产表现出兴趣时，用于通知物业经理的电子邮件模板

2.在模型`realtor.property` 上定义一个方法`action_interested(...)` ，并将其绑定到标签为‘我感兴趣’的按钮上。此按钮在 realtor 表单视图中定义。

3.对于方法`action_interested(...)`的逻辑，我们将实现接下来的步骤。

4.获取对邮件模板上的`body_html`字段的引用，并用动态确定的值替换模板中所有已定义的占位符。模板中的占位符是包装在`--`中的变量。以上模板的例子是`--property-name--`、`--customer-name--`等。

5.接下来，创建一个`mail.mail`模型的实例，传递修改后的邮件模板的`body_html`。

6.最后，在`mail.mail`的实例上调用`send(...)` 方法，将电子邮件发送给房地产经纪人或经理

使用 action_interested(…)方法的房地产属性的模型定义，它发送一封电子邮件通知属性经理对他们的任何属性感兴趣的客户

# 理想的实现

这种替代实现利用了委托设计模式。这种模式简化了开发人员的代码和代码责任。

在这种模式下，操作被委托(转发)给特定的对象或服务来处理它们的职责，而不是由相关的对象来处理完成这些职责所需的底层机制。

在我们发送电子邮件的理想实现中利用这种模式，我们将专注于构建我们的电子邮件模板，然后调用模板上的`send_mail(...)`方法来传递确定向房地产经纪人或物业经理发送电子邮件的机制的职责，而不是在我们的代码中真正处理它。

让我们看看这个理想实现中所需的步骤。

1.  使用 [QWeb 模板定义一个电子邮件模板。](https://www.odoo.com/documentation/16.0/developer/reference/frontend/qweb.html)

当潜在客户对他们的任何房地产表现出兴趣时，用于通知物业经理的电子邮件模板

2.在模型`realtor.property` 上定义一个方法`action_interested(...)` ，并将其绑定到在房地产经纪人表单视图中定义的标记为“我感兴趣”的按钮。

3.对于方法`action_interested(...)`的逻辑，我们将实现下面的步骤。

4.获取电子邮件模板的参考。

5.调用 email 上的`send_mail(...)`方法，使用我们的模板将电子邮件的发送委托给`mail.mail`

使用 action_interested(…)方法的房地产属性的模型定义，它发送一封电子邮件通知属性经理对他们的任何属性感兴趣的客户

比较差的实现和我们理想实现的`action_interested(...)`,很明显后者更简单。它只需要两行代码，而前者需要 10 多行代码。

在 [GitHub](https://github.com/ofelix03/odoo-realtor/tree/ideal-implementation) 上找到完整的源代码。[库](https://github.com/ofelix03/odoo-realtor/tree/ideal-implementation)包含两个实现的基本 Odoo 模块。

不良实施在分支[上不良实施](https://github.com/ofelix03/odoo-realtor/tree/poor-implementation)，而[理想实施](https://github.com/ofelix03/odoo-realtor/tree/ideal-implementation)在分支实施上。

为了测试你自己的模块，通过克隆库并添加路径到你的 Odoo addons 路径库。两者都可以工作，但是理想的实现用更少的代码行实现了相同的功能。

记得检查到您想要测试的实现的正确分支。

[](https://github.com/ofelix03/odoo-realtor/tree/ideal-implementation) [## GitHub - ofelix03/odoo-realtor 处于理想实施状态

### 一个基本的 odoo 模块，演示了用 Odoo 发送电子邮件的两种实现。这两种实现都是功能性的…

github.com](https://github.com/ofelix03/odoo-realtor/tree/ideal-implementation) [](https://github.com/ofelix03/odoo-realtor/tree/poor-implementation) [## GitHub - ofelix03/odoo-realtor 实现不佳

### 一个基本的 odoo 模块，演示了用 Odoo 发送电子邮件的两种实现。这两种实现都是功能性的…

github.com](https://github.com/ofelix03/odoo-realtor/tree/poor-implementation) 

# 方法 2 优于方法 1 的原因

理想的实现消除了在差的实现中需要的下列操作。

1.  **设置电子邮件模板中定义的占位符的值。**
    在理想的实现中，电子邮件模板已经保存了对`object`的引用。在电子邮件模板中，`object`引用客户感兴趣的资产。此`object` 授权直接访问信息，如物业名称、物业经理姓名、客户姓名和电子邮件等。
2.  **创建** `**mail.mail**` **的一个实例，并调用其** `**send(...)**` **方法。**
    在理想的实现中，邮件模板的`send_mail(...)`方法将电子邮件的发送委托给`mail.mail`而不直接调用`send(...)`方法。
3.  **样板文件和详细代码**
    复杂的电子邮件模板需要详细的样板文件代码来更新电子邮件模板中的占位符，创建`mail.mail`的实例，最后调用其`send(...)`函数来发送电子邮件。

*感谢您的阅读。我希望这篇文章对你有用。如果有，请花一点时间到* [*订阅*](https://medium.com/subscribe/@ofelix03) *到* [*我的个人资料*](https://medium.com/subscribe/@ofelix03) *接收我每次发表新文章的通知。*

还有一件事，如果你能支持我的写作，我将不胜感激。通过 [*加盟媒介*](https://medium.com/membership/@ofelix03) *与我的* [*推荐链接*](https://medium.com/membership/@ofelix03) *，我从你的会员费中获得一小笔佣金，这有助于我投入时间和精力来撰写和发表这些有用的文章。*

[](https://ofelix03.medium.com/subscribe) [## 每当费利克斯·奥托发表文章时，就收到一封电子邮件。

### 每当费利克斯·奥托发表文章时，就收到一封电子邮件。通过注册，您将创建一个中型帐户，如果您还没有…

ofelix03.medium.com](https://ofelix03.medium.com/subscribe)