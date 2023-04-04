# lex & Kendra——将企业搜索引入您的机器人的内置意图

> 原文：<https://blog.devgenius.io/lex-kendra-built-in-intent-that-brings-enterprise-search-to-your-bot-ab78dffd695b?source=collection_archive---------2----------------------->

![](img/e00cd14fe715b3f07fe9edab22fb6f58.png)

照片由 [Paramdeo Singh](https://unsplash.com/@oedmarap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

你好。！当创造机器人时，有一种让它们变得更聪明的持续冲动。但是为了做到这一点，我们经常会创建许多意图，这样我们就能够提供各种各样的响应。这是一项单调乏味的任务，经常让开发人员感到困惑。很多时候，我们最终创造了一个意图网来迎合随着时间的推移变得无法控制的流动。

所以现在，考虑一个机器人，你有一个非常具体的目的(问候等)有限的意图集。)以及服务于任何非特定话语的意图。因此，如果用户说“你好”，机器人会回答“你好，我能为你做什么？”但是当用户问‘公司允许多少种假期？’，机器人提供特定的答案或政策文件的摘录。所有这些都可以用自然语言来完成，这意味着你不必使用你喂给 Kendra 的那句话。

这将使您作为开发人员的工作变得非常容易。您可以向 Kendra 提供各种数据，然后用它来提供不同类型的答案。

## 肯德拉:

那么什么是肯德拉？这是一个由机器学习驱动的企业搜索服务。它具有自然语言搜索能力，支持结构化和非结构化数据。这意味着您可以处理特定的 FAQ 类型数据或上下文答案。

## Lex 的新特性:

Aws 已经发布了一个新的内置意图，这使得 Kendra 与 Lex 的集成变得非常容易。

> 为什么要等呢，让我们从设置 Kendra 开始:

转到您的 Kendra 仪表板，开始创建一个索引

![](img/f6968b128e8574fc35ef03aed7838599.png)![](img/3f68782a23cd0c2ed7780647661d5cf6.png)

根据您的需求选择一个版本

![](img/d29d854b33a6b20969a4241ec713f9f4.png)

单击“创建”后，该过程将开始，需要一些时间才能完成。

![](img/b24a99201a4333c71e43ba48d99c17a0.png)

一旦创建了索引，就需要将数据加载到索引中。我把我的文件装进了 S3 桶。我在这里得到了一份休假政策[的样本，并为一名“合同工”创建了自己的常见问题解答，如下所示。](https://www.hrhelpboard.com/hr-policies/leave-policy.htm)

![](img/62098d3ee2b119c72a40b2b22769ea5e.png)

我创建了两个 S3 桶。常见问题文档的“我的-常见问题”和政策文档的“我的-政策-文档”。

![](img/e71b2049fb7b614d7548ca0971f408bd.png)![](img/47b39ecf8aa7954713ccd272c62f0c84.png)

转到索引中的数据源，选择您的 S3 存储桶。

![](img/81c2593e15519f07809411146099d350.png)![](img/f205c231f2334d674765ba7bf7f7b54f.png)![](img/4c40e3af5f229bd0bfeac6c58a494442.png)

继续，在最后一步单击“创建”

![](img/afe0760c3e9e7b965b3488a65d5bf113.png)

现在同步您的数据源。

![](img/c5bb43ff36bfa255cfd49bdc375d9270.png)![](img/df5e4712440a6a725b5340628f38ebc1.png)

我们已经成功地为上下文搜索创建了一个数据源。这将提供用户提问时上传的文档摘录。

现在，让我们创建一些常见问题解答，给出具体的答案。转到 Kendra 中的常见问题部分

![](img/0b3893d5996f0caf96c12a7a585e4e46.png)![](img/9815782cb0545d082c084a4a4b89ec3c.png)![](img/01edec4792cf454b46f2e94f4c5461c3.png)

你已经完成了对肯德拉的设置。从左侧窗格转到“搜索控制台”运行示例搜索

![](img/44ca16eda85608acf4eaf0f2708f38d7.png)

这里有一个上下文搜索的例子。Kendra 从文档中挑选它认为是正确答案的部分。请注意，没有选择 FAQ 答案。

![](img/dc24ee12d925c50bb76fddd5c3a2afcd.png)

但是，如果您键入类似 FAQ 的内容，它会返回一个特定的答案

![](img/2896fd1b4d4267a189cd574fe1cd9f5e.png)

> 如你所见，当被问到一个问题时，Kendra 会提供文档部分或特定的答案。这是由 NLP 和机器学习驱动的。
> 
> 现在，我们将把它与我们的机器人集成，并尝试过滤我们的答案。
> 
> 让我们现在创建我们的机器人。转到 Lex 仪表板。

![](img/7e147f6c388ed1a80a5ca23f93099738.png)![](img/cfa4ef1a6fcfb998bae67a5992e0bc01.png)

让我们创建几个意图。我们将从创建肯德拉的意图开始。

![](img/674373a289ba1bcae5d54772c9297629.png)

选择“搜索现有意向”

![](img/9e0d51ca66a560531ddfbcc494ffa635.png)![](img/b32f8f374296c4d9e8dfa911ffb6f8d8.png)![](img/61198ff27c4d30a9a1a7afcf461133d3.png)

以下是意图的设置。

![](img/fd39f89ffc24ee3721af45f9b1a818a1.png)

Kendra 返回 4 种类型的回答:
>常见问题

>常见问题解答

>是 Kendra 建议答案的文件摘录

>给出与话语相关的另一个答案的文档摘录

因此，请配置您的响应选项卡，依次包含所有这些内容。下面是这些语句和一个显示如何配置它们的图像。

> # FAQ:((x-amz-lex:Kendra-search-response-question _ Answer-question-1))|答案:((x-amz-lex:Kendra-search-response-question _ Answer-Answer-1))。
> #文档摘录:((x-amz-lex:Kendra-search-response-Document-1))。
> #可能答案:((x-amz-lex:Kendra-search-response-answer-1))。

![](img/300b42eb62c1ffc6ccc9691514bd8fa3.png)

保存意图。继续使用以下设置创建另一个名为“greeting”的意图。

![](img/098c9c2006678e64c77168fae3d14594.png)

保存意图并构建机器人。

![](img/7cff08a7372c3f806c25227f3483dd09.png)

> 现在让我们测试机器人。您会注意到它返回任何匹配的响应。*配置响应的顺序无关紧要。*

这里有一个返回常见问题解答的示例

![](img/46676cec8306087f8cabf4919b2a4869.png)

下面是一个示例，虽然首先配置了摘录选项，但它返回了一个可能的答案。如果你在 Kendra 中搜索，你可以看到摘录是可用的。

![](img/a9017de1c14d9d3d2e0e6fa9ae74eea3.png)

> 因此，如果您想获得特定类型的响应，只需在 response 部分输入该值。
> 
> 如果您想在一个订单中选择响应，编写一个 fulfillment lambda 来对您的响应订单进行排序(例如，如果没有找到 faq，选择摘录，等等)

**点击** [**此处**](https://github.com/rohitsharma23/Lex-Kendra-integration-lambda) **为一个样品完成λ。添加您自己的 if-else 条件，以便对答案进行优先排序。**

> 如果您想根据某些属性过滤掉响应，请查看下面的部分。

如下所示，为您的意图添加一个过滤器

> { " equals to ":{ " key ":" _ source _ uri "，" value ":{ " string value ":" Contract " } } }

![](img/caf8715fdc15b2658b0ccf32a5ce9c1e.png)

此筛选器只允许常见问题，因为它们在源 uri 字段中有“合同”。

现在，让我们搜索。

![](img/d4b92d55e180f9f47a5996e98d481631.png)

1 中的搜索有效，因为有 FAQ 答案。2 中的搜索失败，虽然有一个可能的答案，但我们已经根据 FAQ 答案进行了筛选。

这种内置的意图非常有用，可以用一个意图来回答各种各样的问题。您可以搜索成千上万的文档，并有效地向客户提供内容。