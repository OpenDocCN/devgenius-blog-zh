# 如何编写干净的服务器端代码(并节省 30%的代码)

> 原文：<https://blog.devgenius.io/how-did-i-build-my-own-orm-for-mysql-with-python-inspired-by-nestjs-cf56cbaee7fd?source=collection_archive---------11----------------------->

不使用 ORM 的人可能对下面的描述很熟悉——SQL 查询嵌套在服务器代码中的任何地方……并且后端中几乎每两个函数中就有一个 SQL 查询😶。

这也是我们在 Impax 的一些情况，我将在这里向你描述它造成的痛苦，以及我们如何解决它的步骤。

如果你正面临类似的挑战，你会发现这篇文章很有帮助。

![](img/50c31a945ace84e35f0e1aa5f2774dd3.png)

编写嵌套在服务器代码中的 DB 代码是一种众所周知的糟糕做法，它会导致许多棘手问题:

*   大量 SQL 代码重复。例如，在数据库中检查用户是否是测试用户或者是否在不同地方写了超过 30 次的查询。
*   每个 SQL 表修改还需要代码搜索，以找到它在每个服务器文件中的使用位置，了解上下文，实现更改，并测试每个位置的会话。
*   代码文件变得很长，可读性很差，因为 SQL 查询平均有 5 到 20 行，并且存在于每个函数中。

S

我们创建了一个名为“db_services”的新文件夹，db 中的每个表都有一个 python 文件。然后，我们将所有具有 SQL 代码的函数移到这些文件中。

通常，当架构发生变化时，最好不要改变服务器的所有代码，但是每当有新的代码要写时，就用新的方式写。

**步骤 1 的结果**

新的服务器 python 代码可读性更好，也更短，它节省了 SQL 代码中的重复部分，并且更安全，在需要时更容易重构。每当我们有新任务时，我们都会重构 Python 代码来使用这些服务，我们的服务器代码也在变得越来越好。

但这还不够——现在在每个“db_service”文件中，列名都是重复的。考虑一个有 20 列的表，有 INSERT (20 列)、SELECT (20 列)、UPDATE(这 20 列中的一些)函数。这引导我建立第二步。

解决方案第二步——构建我自己的 ORM

我不会在这里详细说明它是如何在幕后构建的(除非您对此感兴趣)，我只会向您展示一个示例来说明它的强大之处。

这是来自“数据库服务”的快照。“user_meeting.py”文件，然后使用 ORM 库:

这是在:

从 GET 函数中，您将获得一个字典，其中包含表中所有列的键值或这些列的列表。注意有 3 个不同的 GET 函数，它们是多么的短和干净(！idspnonenote)。).

不再有 SQL 代码重复

“db_services”文件中再也没有 SQL 查询了。

你可以使用 DTO(它更像是一个 DO，因为我没有用类型来构建它..)类，用于从结果字典中获取所需的特定列。

注意事项:

1.  我知道对于 ORM 来说使用一个库是一个更好的实践，但是我只是在那里转了一圈，并没有花太多时间，而且它对我来说工作得很好。
2.  我可以将 python 类型添加到 DTO 类中，但我不认为我会走得那么远…
3.  这是你能找到那个 ORM 的所有代码的链接:

[https://github.com/israellev/medium-article-orm/blob/main](https://github.com/israellev/medium-article-orm/blob/main/dto_base.py)

感谢您阅读至此！我当然希望收到你的反馈和评论，如果你喜欢这篇文章，你可以鼓掌。

最诚挚的问候！

我邀请你通过链接阅读我的其他文章:

*   [使用 NestJS 和 TypeORM 像专业人员一样记录日志](/logging-like-a-professional-with-nestjs-and-typeorm-dc935f71ef8b?source=friends_link&sk=54014a2dcabc579eb4b0df22e0cab23e)
*   [有效的“干”开发示例(仅适用于懒惰的开发人员！)](https://medium.com/@israellev770/effective-dry-development-example-for-lazy-developers-only-5745df266938?source=friends_link&sk=070c06aec0750fb88b98366c6e9d1f08)