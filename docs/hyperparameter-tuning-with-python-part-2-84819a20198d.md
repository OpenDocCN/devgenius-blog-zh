# 用 Python 调优超参数:第 2 部分

> 原文：<https://blog.devgenius.io/hyperparameter-tuning-with-python-part-2-84819a20198d?source=collection_archive---------3----------------------->

## 实现部分！

![](img/cf17ce851013baa53fd695f3b19ad499.png)

安德里克·朗菲尔德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在本系列博文的[第一部分](https://louisowen6.medium.com/hyperparameter-tuning-with-python-part-1-7aabc622140e)中，我们已经讨论了很多与您在进行超参数调优实验之前需要了解的概念相关的事情，包括:

*   评估机器学习模型
*   引入超参数调谐
*   探索穷举搜索
*   探索贝叶斯优化
*   探索启发式搜索
*   探索多保真优化

在本文中，我们将更多地讨论如何利用几个强大的包来实现前一部分中讨论的所有超参数调优方法。我们将讨论用 Python 调优**超参数一书第二部分的摘要，该书由 4 章组成:**

*   第 7 章，通过 scikit 调整超参数-学习
*   第 8 章，通过 Hyperopt 进行超参数调谐
*   第 9 章，通过 Optuna 调整超参数
*   第 10 章，DEAP 和微软 NNI 公司的高级超参数调整

[](https://www.amazon.com/Hyperparameter-Tuning-Python-performance-hyperparameter-ebook/dp/B0B2DNQHHG/ref=sr_1_1?crid=WHHGESP6AP4M&keywords=Hyperparameter+Tuning+with+Python&qid=1655194083&sprefix=hyperparameter+tuning+with+python%2Caps%2C675&sr=8-1) [## 使用 Python 进行超参数调整:通过以下方式提升机器学习模型的性能…

### Amazon.com:使用 Python 进行超参数调整:通过超参数提升机器学习模型的性能…

www.amazon.com](https://www.amazon.com/Hyperparameter-Tuning-Python-performance-hyperparameter-ebook/dp/B0B2DNQHHG/ref=sr_1_1?crid=WHHGESP6AP4M&keywords=Hyperparameter+Tuning+with+Python&qid=1655194083&sprefix=hyperparameter+tuning+with+python%2Caps%2C675&sr=8-1) 

不再浪费时间，让我们深呼吸，让自己舒服一点，准备学习如何用 Python 实现众多的超参数调优方法！

# 通过 scikit-learn 进行超参数调谐

[Scikit-learn](https://scikit-learn.org/) (Sklearn)是数据科学家使用最多的 Python 包之一。该软件包提供了一系列与机器学习(ML)相关的模块，可以轻松使用，包括超参数调整任务。这个包的一个主要卖点是它跨许多实现类的一致接口，这几乎是每个数据科学家都喜欢的！

除了 Sklearn 之外，还有其他用于超参数调整任务的软件包，它们构建在 Sklearn 之上或模仿 Sklearn 的接口，例如 [scikit-optimize](https://scikit-optimize.github.io/stable/) 和 [scikit-hyperband](https://github.com/thuijskens/scikit-hyperband) 。

在本章中，我们将了解与 scikit-learn、scikit-optimize 和 scikit-hyperband 相关的所有重要内容，以及如何利用它们来实现我们在前面章节中了解的超参数调整方法。我们将学习如何实现以下超参数调整方法:

*   网格搜索
*   随机搜索
*   由粗到细的搜索
*   连续减半
*   超波段
*   贝叶斯优化高斯过程
*   贝叶斯优化随机森林
*   贝叶斯优化梯度提升树

我们将从如何安装每个包开始。然后，我们不仅将学习如何利用这些包的默认配置，还将讨论可用的配置及其用法。此外，我们将讨论超参数调整方法的实施如何与我们在前面章节中学习的理论相关，因为在实施中可能会有一些微小的差异或调整。

最后，借助前几章的知识，您还将能够了解如果出现错误或意外结果会发生什么，并了解如何设置方法配置以匹配您的特定问题。

[](https://scikit-learn.org/stable/) [## sci kit-学习

### “我们使用 scikit-learn 来支持前沿基础研究[...]" "我认为这是我设计过的最棒的 ML 套装…

scikit-learn.org](https://scikit-learn.org/stable/)  [## scikit-optimize:Python-scikit-optimize 0 . 7 . 3 中基于顺序模型的优化…

### 编辑描述

scikit-optimize.github.io](https://scikit-optimize.github.io/stable/) [](https://github.com/thuijskens/scikit-hyperband) [## GitHub-thuijskens/sci kit-hyperband:hyperband 的 scikit-learn 兼容实现

### hyperband 的 scikit-learn 兼容实现。克隆 git 仓库 git 克隆…

github.com](https://github.com/thuijskens/scikit-hyperband) 

# 通过 Hyperopt 的超参数调谐

[Hyperopt](http://hyperopt.github.io/hyperopt/) 是 Python 中的一个优化包，提供了超参数调优方法的几种实现，包括随机搜索、模拟退火(SA)、树形结构 Parzen 估计器(TPE)和自适应 TPE (ATPE)。它还支持各种类型的超参数，具有不同类型的采样分布。

在这一章中，我们将介绍 Hyperopt 软件包，首先介绍它的功能和局限性，如何利用它来执行超参数调整，以及您需要了解的关于 Hyperopt 的所有其他重要信息。我们不仅将学习如何利用 Hyperopt 的默认配置来执行超参数调优，还将讨论可用的配置及其用法。此外，我们将讨论超参数调优方法的实现如何与我们在第一部分(参见本系列博文的第一部分)中学到的理论
相关，因为在实现中可能会有一些微小的差异或调整。

[](https://louisowen6.medium.com/hyperparameter-tuning-with-python-part-1-7aabc622140e) [## 用 Python 调优超参数:第 1 部分

### 在进行超参数调整实验之前，您需要了解的概念

louisowen6.medium.com](https://louisowen6.medium.com/hyperparameter-tuning-with-python-part-1-7aabc622140e) 

在本章结束时，你将能够理解所有你需要知道的关于 Hyperopt 的重要事情，并且能够实现该软件包中提供的各种超参数调整方法。你也将能够理解他们课程中的每个重要参数，以及它们与我们在前面章节中所学理论的关系。最后，掌握了前几章的知识，您将能够理解如果出现错误或意外结果会发生什么，以及如何设置方法配置，使其与您的具体问题相匹配。

 [## 远视文件

### 从 PyPI pip 安装 hyperopt 运行您的第一个示例#定义一个目标函数 def…

hyperopt.github.io](http://hyperopt.github.io/hyperopt/) 

# 通过 Optuna 进行超参数调谐

[Optuna](https://optuna.org/) 是一个 Python 包，提供了超参数调优方法的各种实现，包括但不限于网格搜索、随机搜索和树形结构 Parzen Estimators (TPE)。该软件包还使我们能够创建自己的超参数调整方法类，并将其与其他流行的超参数调整软件包集成，如 [scikit-optimize](https://scikit-optimize.github.io/stable/) 。

在本章中，将向您介绍 Optuna 软件包，从它的众多特性开始，如何利用它来执行超参数调优，以及您需要了解的关于 Optuna 的所有其他重要信息。我们将学习如何实现以下超参数调整方法:

*   树结构 Parzen 估计量(TPE)
*   随机搜索
*   网格搜索
*   模拟退火
*   连续减半
*   超波段

Optuna 有两个主要的类，即采样器和剪枝器。**采样器**负责执行超参数调整优化，而**剪枝器**负责判断我们是否应该根据报告的值对试验进行剪枝。换句话说，修剪程序就像早期停止方法一样，只要看起来继续这个过程没有额外的好处，我们就会停止超参数调优迭代。

> 值得注意的是，Optuna 对连续减半和超带的实现与 scikit 不同。这里，Optuna 将这些方法用作修剪工具，它将支持正在使用的主要超参数调优方法，即采样器。

![](img/efb9f516cce1068b5d2b124a80d37b43.png)

照片由 [Erwan Hesry](https://unsplash.com/@erwanhesry?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

到本章结束时，你将能够理解你需要知道的关于 Optuna 的所有重要的事情，并实现这个包中可用的各种超参数调优方法。你也将能够理解这些课程的每个重要参数，以及它们与我们在前面章节所学理论的关系。最后，借助前几章的知识，您还将能够了解如果出现错误或意外结果会发生什么，并了解如何设置方法配置以匹配您的特定问题。

[](https://optuna.org/) [## Optuna -超参数优化框架

### Optuna 是一个自动超参数优化软件框架，专门为机器学习而设计。它…

optuna.org](https://optuna.org/) 

# DEAP 和微软 NNI 公司的高级超参数调谐

Python 中的分布式进化算法( [DEAP](https://deap.readthedocs.io/en/master/) )和[微软 NNI](https://nni.readthedocs.io/en/stable/) 是 Python 包，它们提供了各种超参数调优方法，这些方法在我们在第 7-9 章讨论的其他包中没有实现，包括:

*   遗传算法
*   粒子群优化
*   墨提斯
*   顺序模型算法配置(SMAC)
*   贝叶斯优化超带(BOHB)
*   基于人口的培训

DEAP 是一个 Python 包，允许你实现各种进化算法，包括(但不限于)遗传算法(GA)和粒子群优化(PSO)。DEAP 允许你以非常灵活的方式设计你的进化算法优化步骤。

 [## DEAP 文档— DEAP 1.3.3 文档

### DEAP 是一个新颖的进化计算框架，用于快速原型和思想测试。它试图使…

deap.readthedocs.io](https://deap.readthedocs.io/en/master/) 

神经网络智能(NNI)是微软开发的一个软件包，不仅可用于超参数调整任务，还可用于神经结构搜索、模型压缩、
和特征工程。

 [## NNI 文档—神经网络智能

### 培训服务 AutoML 实验需要许多试验来探索可行的和潜在的性能良好的模型…

nni.readthedocs.io](https://nni.readthedocs.io/en/stable/) 

在本章中，我们将学习如何使用 DEAP 和微软 NNI 软件包进行超参数调整，从熟悉软件包开始，以及我们需要了解的重要模块和参数。我们不仅将学习如何利用 DEAP 和微软 NNI 的默认配置来执行超参数调整，还将讨论其他可用的配置及其用法。

在本章结束时，你将能够理解你需要知道的关于 DEAP 和微软 NNI 的所有重要事情，并且能够实现这些软件包中提供的各种超参数调整方法。你也将能够理解这些类的每个重要参数，以及它们与我们在前面章节中所学理论的关系。最后，借助前几章的知识，您还将能够理解
如果出现错误或意外结果会发生什么，并理解如何设置方法配置以匹配您的特定问题。

# 你能从书中期待什么

[](https://www.amazon.com/Hyperparameter-Tuning-Python-performance-hyperparameter-ebook/dp/B0B2DNQHHG/ref=sr_1_1?crid=WHHGESP6AP4M&keywords=Hyperparameter+Tuning+with+Python&qid=1655194083&sprefix=hyperparameter+tuning+with+python%2Caps%2C675&sr=8-1) [## 使用 Python 进行超参数调整:通过以下方式提升机器学习模型的性能…

### Amazon.com:使用 Python 进行超参数调整:通过超参数提升机器学习模型的性能…

www.amazon.com](https://www.amazon.com/Hyperparameter-Tuning-Python-performance-hyperparameter-ebook/dp/B0B2DNQHHG/ref=sr_1_1?crid=WHHGESP6AP4M&keywords=Hyperparameter+Tuning+with+Python&qid=1655194083&sprefix=hyperparameter+tuning+with+python%2Caps%2C675&sr=8-1) 

这本书由 14 章组成，分为 3 个部分。这本书涵盖了你需要知道的关于超参数调谐的所有重要的事情，从概念和理论，实现，以及如何把事情付诸实践开始。除了对每种方法如何工作的深入解释之外，您还将使用一个 ***决策图*** 来帮助您确定满足您需求的最佳调优方法。这本书还附有代码实现，您可以从 GitHub 免费获得！

[](https://github.com/PacktPublishing/Hyperparameter-Tuning-with-Python) [## GitHub-packt publishing/Hyperparameter-Tuning-with-Python:Hyperparameter Tuning with Python

### 这是用 Python 进行超参数调优的代码库，由 Packt 发布。促进您的机器学习…

github.com](https://github.com/PacktPublishing/Hyperparameter-Tuning-with-Python) 

本书面向使用 Python 的数据科学家和机器学习工程师，他们希望通过利用适当的超参数调整方法来进一步提高 ML 模型的性能。您需要对 ML 和如何用 Python 编码有一个基本的了解，但是不需要预先了解 Python 中超参数调优。

# 关于作者

Louis Owen 是一名来自印度尼西亚的数据科学家/人工智能工程师，他总是渴望新知识。在他的职业生涯中，他曾在多个行业领域工作，包括非政府组织、电子商务、对话式人工智能、OTA、智能城市和金融科技。工作之外，他喜欢花时间帮助数据科学爱好者成为数据科学家，要么通过他的文章，要么通过辅导会议。他还喜欢在业余时间做自己的爱好:看电影和做兼职项目。

目前，Louis 是世界领先的 CX 自动化平台 Yellow.ai *、*的 NLP 研究工程师。查看[路易斯的网站](http://louisowen6.github.io/)，了解更多关于他的信息！最后，如果您有任何疑问或需要讨论的话题，请通过 [LinkedIn](https://www.linkedin.com/in/louisowen/) 联系 Louis。