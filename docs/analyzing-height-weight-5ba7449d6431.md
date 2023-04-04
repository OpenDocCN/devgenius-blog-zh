# 分析身高和体重

> 原文：<https://blog.devgenius.io/analyzing-height-weight-5ba7449d6431?source=collection_archive---------10----------------------->

## 通过贝叶斯分析

因此，我开始了一个系列，在这个系列中，我将浏览几个不同的数据集，并通过贝叶斯分析对它们进行分析。这不是第一个例子，因为我之前已经发布过关于[非线性](https://towardsdatascience.com/in-modelling-the-first-steps-are-the-hardest-a4250b80a0f2)和[纵向](https://medium.com/mlearning-ai/mixed-models-of-chicks-weight-a41413c99b51)贝叶斯的例子。

在这个例子中，我将使用一个随机生成器，这里创建的[是](https://github.com/boboppie/kruschke-doing_bayesian_data_analysis/blob/master/2e/HtWtDataGenerator.R)，用三个参数制作一个数据集:*身高*、*体重*和*性别*。这个数据集的好处是，尽管它们之间的关系有一种直观的吸引力，但我们可以指定一个完全不符合我们先前假设的数据集，和/或创建一个“奇怪”但可行的先验。

定义先验是贝叶斯分析中最重要的部分，它本身需要实验设计。因此，支持先验是纯粹的研究，虽然我在这里没有对可行的先验做任何研究，但我确实使用了信息性先验。也就是说，我决定变量之间的关系必须是一个给定的函数，我支持它，没有使之前的分布中的方差变大。

因此，我使用信息性的先验，我支持使用它们是为了向你展示它们的影响，而不是最可行的先验，这需要通过已知的文献对身高、体重和性别进行彻底的分析。

好，我们开始吧。

![](img/88296f9f3895bbb07d99409ac4f7df53.png)

数据生成器的前六行。

![](img/67b4f3f8f43d21c20c4c8fc89235602c.png)![](img/a2f90d48740a3216baf95f59718f7405.png)

绘制生成的数据。

![](img/eb34cb8b5b9ba95b44cae92b7fb4a962.png)

和三个线性回归模型的结果。

![](img/8e476edcad98ea9ddd54f55456bf35f1.png)

模型与上一个模型的曲线相吻合。

![](img/ef8f484e465827d33b0bb6d19c4b58fb.png)

最后一个模型的置信区间。

![](img/6ae05a4707240acce608d44e1bf074b6.png)

以及三个嵌套模型的 ANOVA 比较。如您所见，最后一个模型**模型 fit2.2** 的性能最好，这应该是因为它最符合数据生成流程。

让我们建立一个贝叶斯模型。因为几乎所有的 R 包都查看 STAN 编译器的贝叶斯抽样，所以也很高兴看到用 R 编写的代码是如何发送给 STAN 的。你可以在下面看到，使用一个没有先验，只有数据的模型。

![](img/ba097dd66c3efeeb13a65eec90df382c.png)

**stancode** 函数展示了 **brms** 包如何将 R 代码转换成 **STAN** 代码，这是 STAN 编译器工作所必需的。

![](img/43c33817b4578e60669a7b7057cd5431.png)![](img/12bb50f05aa1cbd88ce4478a2bda15ea.png)

样品看起来不错——链条不错，而且变化多样。

现在，以上是没有什么壮观的。考虑到我们没有指定任何先验，这只是可能性分析。

![](img/6050aee133371169ce695102d501fbbb.png)

**brms** 中的一款新车型，有着广泛的前科。然而，正如你将看到的，它们偏离了数据，因为我所做的是说明，平均来说，我认为身高和体重之间没有任何关系。

![](img/c8dbf2cfc44322e174141c7c000670b8.png)

模型拟合

![](img/fbc63529b997676b32deb4ceab6b0dcd.png)![](img/a07456d1bf6218f1ed7a32c351762e6e.png)

并且可能性( **y** 和后验概率( **yrep** )之间的偏差非常明显。没什么好担心的，这是科学。

![](img/97833d17b5f45f2bb7563a392d509a8e.png)![](img/c311d6388ec26ae3b86ded3a25afbea5.png)![](img/e4416e47b7faed04b8b2a00bfc962d6c.png)

可以看到 **yrep** 的采样很漂亮，但是离 **y** 很远。这就是当你的先验和可能性大大偏离时你所得到的。再说一次，没什么好担心的，这就是科学的工作方式。

![](img/fd0fd9cb57890c468187d1038b0ff4f7.png)

这个阴谋将动摇大多数人，怀疑他们的先验。但是，如果我可以证明，我相信身高和体重之间没有关系，先验是没问题的，所以我们有一些真正有趣的东西。当然，身高和体重肯定是有关系的。但是，如果我能为他们为什么没有辩护，我就在科学的核心——知识积累上移动。

现在，让我们更进一步，让先验知识更具知识性和方向性。

![](img/d84853c05038436543b46183ca28c813.png)![](img/71750c77d9df15a68c4bb9d614747b92.png)

包括 **brms** 型号和**标准**代码。

![](img/61854da0558b98e203914008a409bf72.png)

取样顺利

![](img/abbbb21edf55393fd80912185409f7cb.png)![](img/6d281df16c0612479bdfa236efbffef4.png)

条件概率-一个用于连续预测值，一个用于分类预测值。

现在，你们中大多数查过贝叶斯分析的人可能也读到过，只有遵循贝叶斯分析，才能说来自模型的值的区间可以真实地陈述:“在 XX%的情况下，变量 Y 的值将落在这个范围内”。这就是我们所说的可信区间，考虑到它是贝叶斯分析的基础，它应该有自己的博客文章。然而，现在，我将只向您展示，当指定无信息先验时，当使用[等尾可信区间](https://rdrr.io/cran/bayestestR/man/eti.html#:~:text=Description,as%20Credible%20Interval%20(CI).)时，[置信度](https://medium.com/mlearning-ai/inference-estimates-p-values-and-confidence-limits-a-frequentist-approach-acdd45d94bd5)和[可信区间](https://easystats.github.io/bayestestR/articles/credible_interval.html)几乎相等。不使用[最高密度区间](https://www.sciencedirect.com/topics/mathematics/highest-density-interval#:~:text=Second%20Edition)%2C%202015-,4.3.,highest%20density%20interval%2C%20abbreviated%20HDI.&text=The%20HDI%20indicates%20which%20points,cover%20most%20of%20the%20distribution..)时。

![](img/0a3f991a5e5258e84a8fd18f7e2ce074.png)

当要求等尾区间时，置信区间和可信区间重叠。

![](img/0a007356ade3ba25dcfc6de3d42b0c36.png)

从模型中抽取的前六个后位。

![](img/ed4f25fd46667031407f5eb91300ac6c.png)

回归线来自后半部分，点来自女性的可能性。

![](img/f58c7ff67f2e99399016140d9419e64b.png)

回归线来自后验概率点来自男性。

![](img/74a671780cb66d370ee99f1dc7d2b131.png)

平均后验和后验来自数据之上的贝叶斯模型。

![](img/0061de96ebf21bf9a70160eea07448d1.png)

以上所有情节结合起来。

现在，让我们再次回顾一下最高密度区间(HDI)。我下面做的是绘制 sigma 变量的分布，这是均值= 0 的残差的标准偏差。直方图是 85%的 HDI，海绿色覆盖图是 95%的置信区间(CI)。黑点和绿点是它们各自模式和含义。

人类发展指数表明分布的哪些点是最可信的，哪些点覆盖了分布的大部分。对于正态分布，由于正态分布的对称性，HDI 和 CI 通常会重叠。

![](img/5175cc12595ca4dda3b523f16eb23a56.png)

最后，但同样重要的是，让我们看看完整的蒙蒂:置信区间和预测区间，以及可信区间。下面是来自模型预测的数据。

![](img/f315a9daa87bed42214e5dfe4aa9bd0c.png)![](img/844d98365eb58a4dec88f9324f0a0615.png)

如你所见，置信区间和预测区间是完全不同的。置信区间表示参数的不确定性，预测区间表示单个数据点的不确定性。

![](img/0839d055ddf6289a19c86f2e6330eeb7.png)![](img/b62bb2ed6a7f21c9b6c2e699394f2357.png)

来自贝叶斯模型的置信区间。

好吧，我希望这是一个很好的例子，让你们可以学习。更多将在未来几天发布！如果有什么不对劲，请务必让我知道！

在代码下面。