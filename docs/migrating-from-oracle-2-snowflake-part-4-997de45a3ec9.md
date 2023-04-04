# 从 Oracle 2 雪花迁移:第 4 部分

> 原文：<https://blog.devgenius.io/migrating-from-oracle-2-snowflake-part-4-997de45a3ec9?source=collection_archive---------2----------------------->

![](img/fb389d3d5acc3029752a29d804835779.png)

这篇博客是我上一篇博客**从 Oracle 2 雪花迁移:第 3 部分**的延续，我想继续分享我从 Oracle 迁移到雪花的经历。

**注意:这更多的是为了让人们了解在 Oracle 到雪花迁移过程中会面临哪些挑战。所有建议都是我个人的，与我的雇主(过去/现在)无关。**

现在，如果您正在关注我之前的 3 篇博客，那么您已经清楚我们希望如何进行对象和数据迁移。

现在是时候为日常处理设置我们的申请流程了。因为我们应用程序依赖于在不同时间间隔从源系统传入的文件，所以我们与源应用程序是松散耦合的。即使我们几天不处理收到的文件，我们也能生存下来，因为我们不是面向客户的主动应用程序，而是我们的数据更多地用于决策。

正如我们所提议的，新的架构使用 Azure blog 和 Snowflake snowpipe 来处理所有的增量负载。我们用事件中心设置创建了一个输入文件夹。在雪花端，我们创建了 Snowpipe 来监听存储事件触发器。我们设置了文件格式、外部阶段、blob 安全性，并准备好在 ETL (Python)将数据放到 Azure blob 上时接收数据。早期的 PERL 脚本更多地用于文件处理和 SP 调用，因为业务逻辑保存在 Oracle 数据库中。在迁移到雪花的过程中，我们利用 PYTHON 编程语言将业务逻辑从数据库推到 ETL 层。总之，我们通过 ETL 层利用这个逻辑，所以我们不会面临任何行为上的差异。

我们所有的代码更改都是 Azure Repo GIT 的一部分，我们所有的 CI/CD 都是使用 Azure 管道设置的。

在上线当天，我们冻结了现有的应用程序处理，我们已经预先转换了表 DDL，并使用 Azure 管道将其部署在雪花中。使用我们的本地方法加载海量数据。数据加载后开始数据验证。一旦数据得到验证，我们就安排我们的 ETL(Python)脚本开始从源路径获取数据，并将其推送到 Azure blob 存储。

奇迹开始了，事情开始像我们计划的那样运转。

现在，当我写这篇博客的时候或者当你读这篇博客的时候，这可能感觉很简单，但是将需要的步骤分开并保持正确的顺序总是具有挑战性的。我必须感谢我的团队在这方面的努力工作，并帮助我完成这个项目。

希望这篇博客能帮助你了解我们如何从 Oracle 迁移到雪花。如果你对此有任何疑问，欢迎在评论区提问。如果你喜欢这个博客，请鼓掌。保持联系，看到更多这样的酷东西。谢谢你的支持。

**你可以找到我:**

**跟我上媒:**【https://rajivgupta780184.medium.com/ 

在推特上关注我:https://twitter.com/RAJIVGUPTA780

**在 LinkedIn 跟我连线:**[https://www.linkedin.com/in/rajiv-gupta-618b0228/](https://www.linkedin.com/in/rajiv-gupta-618b0228/)

**订阅我的 YouTube 频道:**[https://www.youtube.com/channel/UC8Fwkdf2d6-hnNvcrzovktg](https://www.youtube.com/channel/UC8Fwkdf2d6-hnNvcrzovktg)

![](img/b6f53bd4f35a4c4b2c529865ca887f9c.png)

#坚持学习#坚持分享#每天学习。

# 参考资料:-

*   [https://www.snowflake.com/](https://www.snowflake.com/)