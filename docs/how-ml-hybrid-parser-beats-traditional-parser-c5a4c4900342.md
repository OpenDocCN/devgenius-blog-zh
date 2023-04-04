# ML 混合解析器如何击败传统解析器

> 原文：<https://blog.devgenius.io/how-ml-hybrid-parser-beats-traditional-parser-c5a4c4900342?source=collection_archive---------17----------------------->

[机器学习](https://serpapi.com/blog/tag/machine-learning/)

![](img/6e348569d4e5f236f2ddcfd28f312701.png)

# 介绍

这是与人工智能实现相关的系列博文的一部分。如果你对故事的背景或情节感兴趣:

[#1)如何用人工智能抓取 Google 本地结果？](https://serpapi.com/blog/how-to-scrape-google-local-results-with-artificial-intelligence/)
[# 2)Rails 上机器学习的真实世界示例](https://serpapi.com/blog/real-world-example-of-machine-learning-on-rails/)
[#3) AI 训练技巧和比较](https://serpapi.com/blog/better-training-tips-and-comparisons/)
[# 4)Rails 中机器学习](https://serpapi.com/blog/machine-learning-in-scraping-with-rails/)
[#5)在 Rails 中实现 ONNX 模型](https://serpapi.com/blog/implementing-onnx-models-to-rails/)
[# 6)ML 混合解析器如何击败传统解析器](https://serpapi.com/blog/how-ml-hybrid-parser-beats-tradition/)

本周我们将分享与我们有问题的解析器的比较，以及`ML-Hybrid`模型将如何改进我们的解析。我选择了 [SerpApi 的 Google Local Pack Scraper API](https://serpapi.com/local-pack) 作为试验场。我们将只在比较的范围内使用`Desktop`结果

# 比较规则

让我们定义一些规则来比较两个解析器:

*   正确的解析给`+3`加分
*   不正确的解析给了`-2`分
*   部分正确的解析仍然是不正确的解析，所以`-2`分
*   如果一个正确解析的值出现在另一个键上，第二次出现将被单独考虑`-2`，所以最后它将等于`+1`(减号表示混淆)
*   如果一个以上的元素出现在一个不需要的键上，它们将分别有`-2`点
*   如果 HTML 部分中的文本没有被抓取到任何一个键中，那么它会因为没有引起混乱而得到`-1`分

# 例子

为了消除比较中的偏差，我们尝试使用来自不同业务部门的不同查询的查询。计算`ML-Hybrid Model`没有任何问题。然而，读者必须考虑到`Traditional Parser`的无效性，不同的值会出现相同的问题。也就是说，传统的解析器使用大量复杂的条件来解析结果。很难维持，CSS 目标几乎每两周就要改变一次。另一方面，`ML-Hybrid`模型使用简单的 CSS 目标，如`div:has(text())`，以便收集目标字段中必要的文本。

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:牙医
`Traditional Parser Score` : -10
`ML-Hybrid Parser Score` : 14

![](img/cd52daec3ea4458656f4068e06184f0a.png)

传统解析器，总分:-10

![](img/cf14dd5038d0c193800631c492699056.png)

混合语法分析器，总分:14

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:牙医
`Traditional Parser Score` : -10
`ML-Hybrid Parser Score` : 10

![](img/4eba02edf1c90cec80463f62b0e359d0.png)

传统解析器，总分:-10

![](img/91c6ff09b09e927273f2c0fa313a4298.png)

混合解析器，总分:10

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:牙医
`Traditional Parser Score` : -8(全部不正确)
`ML-Hybrid Parser Score` : 1

![](img/535886a2142f026f42e34a43bed0f63a.png)

传统解析器，总分:-8(全部不正确)

![](img/e4173dba81e7752c0411dd777fc7317d.png)

ML-混合解析器，总分:1

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:书店
`Traditional Parser Score` : -10
`ML-Hybrid Parser Score` : 14

![](img/c1abb6b091d9cc8866ee71cf171ba921.png)

传统解析器，总分:-10

![](img/13100facb25061d4bc7daab631dc66c6.png)

混合语法分析器，总分:14

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:书店
`Traditional Parser Score` : -10
`ML-Hybrid Parser Score` : 21(全部正确)

![](img/438e79df8bade7fed624ad5444a90f66.png)

传统解析器，总分:-10

![](img/a5c3c92e58295ca7655bfa69691280df.png)

ML-混合解析器，总分:21(全部正确)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:书店
`Traditional Parser Score` : -3
`ML-Hybrid Parser Score` : 17

![](img/152873da03f438cc9c63e67e1357bb64.png)

传统解析器，总分:-3

![](img/6c97d60174b93e3499091ee55a7bf65a.png)

混合解析器，总分:17

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:墨西哥餐厅
`Traditional Parser Score` : -9
`ML-Hybrid Parser Score` : 15(全部正确)

![](img/12c687a8dd00dc03617aa3d31e03e855.png)

传统解析器，总分:-9

![](img/fe92de6bd85b3f591108a34b1ba35dea.png)

混合解析器，总分:15

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:墨西哥餐厅
`Traditional Parser Score` : -7
`ML-Hybrid Parser Score` : 12(全部正确)

![](img/2abcc0346144df5bd422530ece9b6fef.png)

传统解析器，总分:-7

![](img/f056c8b69881e53fc18cafb260a83cc8.png)

ML-混合解析器，总分:12(全部正确)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:墨西哥餐厅
`Traditional Parser Score` : -7
`ML-Hybrid Parser Score` : 12

![](img/1750cbdd253a5372afaee1525875eb43.png)

传统解析器，总分:-7

![](img/a1cf025d01a8235a2f054a64a19502ad.png)

混合解析器，总分:12

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:保险代理
`Traditional Parser Score` : -9
`ML-Hybrid Parser Score` : 15(全部正确)

![](img/6896771b541103dd3bcff41a81c0e144.png)

传统解析器，总分:-9

![](img/dd11359df3dccd2ae612fef122e502f0.png)

ML-混合解析器，总分:15 分(全部正确)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:保险代理
`Traditional Parser Score` : -6
`ML-Hybrid Parser Score` : 11

![](img/9b43846ae81b63d95794771fd59f4647.png)

传统解析器，总分:-6

![](img/f1550dc237c73e421ee198ac9885c14a.png)

混合解析器，总分:11

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:保险代理
`Traditional Parser Score` : -9
`ML-Hybrid Parser Score` : 12(全部正确)

![](img/57a7e66f4d90a22a3a63664b431f83d6.png)

传统解析器，总分:-9

![](img/95db6edc11b73f983d552d9d9f7aab46.png)

ML-混合解析器，总分:12(全部正确)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:咖啡
`Traditional Parser Score` : -9
`ML-Hybrid Parser Score` : 15(全部正确)

![](img/3e060eb97db9b75b4740f7ceb01bddd2.png)

传统解析器，总分:-9

![](img/e0881527dbb7d665a462e6334a17d75e.png)

ML-混合解析器，总分:15 分(全部正确)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:咖啡
`Traditional Parser Score` : -12
`ML-Hybrid Parser Score` : 15(全部正确)

![](img/751643432fe886af870dcfa04cc5824d.png)

传统解析器，总分:-12

![](img/7ec2d51a973be437bfe5a738b717b0c6.png)

ML-混合解析器，总分:15 分(全部正确)

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

查询:咖啡
`Traditional Parser Score` : -8
`ML-Hybrid Parser Score` : 18(全部正确)

![](img/09c2fde885481afea53a357c8560a6ba.png)

传统解析器，总分:-8

![](img/94a5b55aaeb1d9550dea0f44cd84ae82.png)

混合解析器，总分:18

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

# ML-Hybrid 解析器是如何工作的？

首先，我必须声明，我还没有扩展用于支持解析器后面的`n-gram classifier`模型的词汇表。我想看看它在低级别数据馈送下的表现，以及它的去向。这意味着它可以在未来得到改善。

有助于形成 ML 模型的两个核心是:

*   它可以预测单个字符串
*   它可以决定一个字符串是否应该从分隔符中分离出来(像传统的解析器一样处理轻微的偏差情况)

它决定一个字符串是否应该被分割的方法很简单:

*   它从指定的点分割字符串，并获得每个分割文本的预测的最大输出概率。这是什么意思:
    这意味着如果你给模型一个字符串，它会给你一个由不同浮点数组成的数组形式的输出。数组的大小是您想要预测的目标键的数量。这个数组由表示概率值的浮点数组成，这些浮点数中最大值的索引将给出您想要预测的关键字。然而，现在我们感兴趣的是这些浮点数的最大值。
    例如:5540 N Lamar Blvd #12 的输出= [0.6，0.4]
    德克萨斯州奥斯汀 78756 的输出= [0.8，0.2]
*   模型取这些最大概率值的平均值
    例如:分离的平均产量= (0.6 + 0.8)/2 = 0.7
*   然后，它取组合文本的最大概率输出
    例如:“5540 N Lamar Blvd #12 ⋅奥斯丁，tx 78756”=[0.9，0.1]的输出
*   最后，它将两者进行比较，以决定拆分文本是否更明智。由于`0.9`大于`0.1`，它将选择不分割文本。请注意，这三篇文章都给了我们`address`的钥匙。

除此之外，与`traditional parser`的主要区别在于它如何首先收集文本。在`traditional parser`中，你有一组明确的目标，在这些目标中，你可以通过不同的条件来解析你需要的元素。`ML-Hybrid`也支持许多正则表达式条件。然而，最终它是一个文本网格，是一个密钥的竞争。它有两个用处:

*   您不必为单个键编写复杂的解析器，初始解析器可能就足够了。
*   您不必更新 CSS 目标或经历各种情况，如果出现新的键，您可以用两个简单的命令更新键并重新训练模型。你不需要用旧钥匙。

# 结论

这篇博文明确证明了`ML-Hybrid`模型可以更高效、更持久地应用于解析目的。感谢读者的关注，感谢`SerpApi`辉煌的人们，感谢他们在新领域扛起大旗的所有支持，甚至在这些`trying times`。下周，我们将讨论如何进一步改进模型，运行时的比较基准，以及`General Testing Purposes for Machine Learning.`

*原载于 2022 年 3 月 9 日 https://serpapi.com*[](https://serpapi.com/blog/how-ml-hybrid-parser-beats-tradition/)**。**