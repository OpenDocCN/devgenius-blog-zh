# 报告的自动化第 2 部分

> 原文：<https://blog.devgenius.io/automation-of-reporting-part-2-6d9c8dbad8a3?source=collection_archive---------18----------------------->

我继续我的分析出版物:

*   [商务智能系统中的数据分析与可视化](https://blog.devgenius.io/@alex.gladkikh/a-data-analysis-in-business-part-1-7f92c6e919e2)
*   [产品指标分析](https://blog.devgenius.io/@alex.gladkikh/analyses-of-product-metrics-dd4ec2df8590)
*   [а/B-试验-第 1/3 部分(AA-试验)](https://blog.devgenius.io/@alex.gladkikh/%D0%B0-b-tests-part-1-3-aa-test-d4eb7b8b1221)
*   [а/B-试验-第 2/3 部分(AB-试验)](https://blog.devgenius.io/@alex.gladkikh/%D0%B0-b-tests-part-2-3-aa-test-9728beaaa709)
*   [A/B-试验-第 3/3 部分(关系指标)](https://blog.devgenius.io/@alex.gladkikh/a-b-tests-part-3-3-relationship-metrics-882afe43f130)
*   [搭建 ETL-管道(气流)](https://medium.com/@alex.gladkikh/building-an-etl-pipeline-airflow-2fd098cd61ae)
*   [报告自动化第 1 部分](/automation-of-reporting-2abe7f101801)

接下来，我们需要收集一份关于整个应用程序运行情况的报告。该报告应包括关于新闻源和消息发送服务的信息。报告应在每天 11:00 到达聊天室。

在第一步中，我们导入所有必要的库并定义必要的变量。我们还定义了我们的机器人的令牌，因为我们将需要它来进行测试。然后我们将它隐藏在 GitLab 上，就像我们在最后一部分所做的那样:

接下来，我们形成一条文本消息，将显示在报告中:

现在，我们需要编写查询来获取所有这些指标的数据。第一个请求将是对提要的请求:

下一个请求将是对信使的请求:

现在平台上有一个大查询，需要两个表中的数据:

现在还需要找到新用户的数量。在此请求中，您需要找到每个用户的最小日期，并将其作为注册日期:

接下来，我们设置变量来处理日期，并为所有数据框架校正日期和数据类型:

现在填写文本模板:

现在我们可以为图形编写一个函数:

在我为图形写了函数之后，我在***send _ telegraph _ report***函数的末尾添加了以下几行，并在最后调用了该函数:

将生成的文件上传到任何系统进行自动化。和上一篇文章一样，我将使用 GitLab CI/CD。我已经有了一个自动化的任务。在这个连接中，我将创建第二个任务，并为这两个任务分配变量，这样就可以在 yml 文件中自动执行它们。应该为旧任务和新任务分配变量。

![](img/6471202307bb26c959c22f0ba25b0df2.png)![](img/05bc3805a207aba55f4b2ca497c9be3d.png)

之后，我对 yml 文件进行修改，就这样。报告将于每天 11.00 和 11.05 发送至我们的小组:

我的小组中的报告作为该算法的操作结果看起来像这样:

![](img/231542168ad9c623553df2003be23f6e.png)![](img/df3f644f05f3ba7ba82f73a409777cc4.png)![](img/d36ac5ca6752914056540269c83c5f19.png)![](img/d8ef000afbba38258628dce7de78b81b.png)

由于所做的工作，我们设法创建了一个自动报告系统，通过电报频道向克虏伯公司提供每日信息。在接下来的最后一篇文章中，我将回顾 alerts 中的工作。

我提醒你，我所有的作品你都可以看看我的网站，如果你需要完整的代码:[https://alex.gladkikh.org](https://alex.gladkikh.org/)。