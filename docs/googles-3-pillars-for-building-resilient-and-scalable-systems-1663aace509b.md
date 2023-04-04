# 谷歌构建弹性和可伸缩系统的三大支柱

> 原文：<https://blog.devgenius.io/googles-3-pillars-for-building-resilient-and-scalable-systems-1663aace509b?source=collection_archive---------6----------------------->

## 使用这些来设计运行在最高水平的系统

如何构建弹性和可伸缩的系统？

这是管理人员、开发人员和技术领导者每天都会问的问题。如果你相信那些向我寻求工作机会的人，有很多人从年轻时就对此充满热情。我以为青少年会谈论足球和八卦，但显然，真正要谈论的是无服务器计算。家长们，记笔记。

![](img/06aa5b151f92044aba8787253ec5849d.png)

照片由[米切尔罗](https://unsplash.com/@mitchel3uo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

幸运的是，谷歌为我们提供了一个很好的答案。还有谁比大 G 更值得期待呢？[毕竟，谷歌每天处理**超过 20 Pb**的数据](https://seedscientific.com/how-much-data-is-created-every-day/#:~:text=Google%20processes%20more%20than%2020,in%20the%20world%20in%202020.)。Google 有三种他们大力推广的模式，目的是为了建立更具伸缩性和弹性的系统。让我们深入了解它们是什么，以及如何改进它们。

AlphaSignal 是人工智能、机器学习和数据科学领域顶级发展的免费每周摘要。他们使用人工智能来排名并向你发送该领域的顶级发展，从而节省你大量的时间。如果你正在寻找有助于跟上机器学习步伐的东西，请查看它们。阅读它们是与这个领域保持联系的一个很好的方式，并且支持我的写作，不需要你付出任何代价。

[](https://alphasignal.ai/?referrer=Devansh) [## 阿尔法信号|机器学习的极品。艾总结的。

### 留在循环中，不用花无数时间浏览下一个突破；我们的算法识别…

alphasignal.ai](https://alphasignal.ai/?referrer=Devansh) 

# 谷歌的三大支柱

1.  **什么是可伸缩系统-** 可伸缩系统是任何能够处理增加的负载的系统。理想情况下，无论您有 20 个用户还是 200 个用户，您都希望您的系统具有相同的性能。请记住，大多数科技行业都是根据规模经济的经济原则运作的(生产越多，产品越便宜)。因此，可伸缩性是系统中的必备要素。您经常会看到解决方案牺牲一些性能来保持可伸缩性。[要更深入地理解这一关键原则，请阅读此](https://codinginterviewsmadesimple.substack.com/p/the-mathematics-of-scale-math-mondays)。
2.  **什么是弹性系统-** 弹性系统是能够应对各种挑战的系统。它能处理用户不断刷新页面吗？有奇怪名字(特殊符号等)的人呢？它能抵御 SQL 注入和其他黑帽企图吗？既然你们中的很多人说你们对人工智能和数据感兴趣，这是机器学习中一个非常重要的研究领域。正在研究的事情之一叫做对抗性学习。这是对这个主题的一个简单介绍

1.  **支柱 1，自动化-** 我很惊讶在这里看到这个，但深入思考，它确实有意义。流程自动化程度越高，系统处理崩溃、高峰、异常和其他导致问题的事件的能力就越强。更多的自动化使您的系统保持运行，即使您的开发人员/管理员不在身边。投资于更多的自动化将帮助您提高系统的可伸缩性和弹性。
2.  **支柱 2，松耦合-** 松耦合指的是将你的模块彼此解耦(减少它们之间的依赖)。这非常重要，因为紧密耦合的系统更有可能完全关闭。一个服务的关闭可能会导致与之无关的服务完全崩溃。促进松耦合非常重要。如果您可以设计松散耦合的系统，那么实现微服务架构可能是一个不错的选择。

[](https://codinginterviewsmadesimple.substack.com/p/monoliths-vs-microservices-system?utm_source=substack&utm_campaign=post_embed&utm_medium=web) [## 单片 vs 微服务[系统设计周日]

### 为了帮助我更好地了解您，请填写这份 2 分钟的匿名调查。如果你喜欢这篇文章，请确保你…

codinginterviewsmadesimple.substack.com](https://codinginterviewsmadesimple.substack.com/p/monoliths-vs-microservices-system?utm_source=substack&utm_campaign=post_embed&utm_medium=web) 

1.  **支柱 3，数据** - **驱动洞察-** 这让我很开心。拥有更多关于您的系统的数据将允许您做出更优化的决策。集成大量日志记录、持续监控和一些分割测试，以获得更好的结果。如果你想通过数据发展自己的技能，请联系我。链接总是在最后。

对于那些想知道更多细节的人，可以在下面找到一个介绍这个概念的视频。这个视频是由谷歌云的开发者参与主管王从希制作的。你可以在 Steph 的 LinkedIn 上联系她。她为开发者提供了很多内容，所以你可能会喜欢她分享的内容。

如果你喜欢这篇文章，你会喜欢我的每日电子邮件简讯[技术使之变得简单](https://codinginterviewsmadesimple.substack.com/)。它涵盖了算法设计、数学、人工智能、数据科学、最近的技术事件、软件工程等主题，让你成为更好的开发人员。 [**我目前正在进行全年八折优惠，所以一定要去看看。**](https://codinginterviewsmadesimple.substack.com/subscribe?coupon=1e0532f2) 使用此折扣会降低价格-

***每月 800 印度卢比(10 美元)→ 533 印度卢比(8 美元)***

***每年 8000 印度卢比(100 美元)→6400 印度卢比(80 美元)***

[你可以在这里了解更多时事通讯。如果您想和我谈谈您的项目/公司/组织，请滚动下方并使用我的联系链接联系我。](https://codinginterviewsmadesimple.substack.com/about)

# 向我伸出手

使用下面的链接查看我的其他内容，了解更多关于辅导的信息，联系我了解项目，或者只是打个招呼。

机器学习重要更新的免费每周总结(赞助)-[https://lnkd.in/gCFTuivn](https://lnkd.in/gCFTuivn)

为了帮助我了解你[填写这份调查(匿名)](https://forms.gle/7MfQmKhEhyBTMDUD7)

查看我在 Medium 上的其他文章。https://rb.gy/zn1aiu

我的 YouTube:[https://rb.gy/88iwdd](https://rb.gy/88iwdd)

在 LinkedIn 上联系我。我们来连线:[https://rb.gy/m5ok2y](https://rb.gy/f7ltuj)

我的 insta gram:[https://rb.gy/gmvuy9](https://rb.gy/gmvuy9)

我的推特:[https://twitter.com/Machine01776819](https://twitter.com/Machine01776819)

如果你想在科技领域发展事业:[https://codinginterviewsmadesimple.substack.com/](https://codinginterviewsmadesimple.substack.com/)