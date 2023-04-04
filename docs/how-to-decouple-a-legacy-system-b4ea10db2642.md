# 如何分离遗留系统

> 原文：<https://blog.devgenius.io/how-to-decouple-a-legacy-system-b4ea10db2642?source=collection_archive---------1----------------------->

## 改进遗留代码的练习

有很多文章解释如何做出好的设计以及遵循什么规则。在本笔记中，我们将看到一个如何将 [*传统设计*](https://en.wikipedia.org/wiki/Legacy_system) *转化为更好设计的具体例子。*

![](img/d170bc993e2bdbac1c4ad85319d19de3.png)

照片由[丹尼尔·福克斯](https://unsplash.com/@danieldotfox?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/rust?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 问题是

许多现有系统存在耦合问题。因此，它们的可维护性降低了。改变这种类型的系统会带来巨大的连锁反应。

[](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [## 耦合:唯一的软件设计问题

### 对我们软件的所有故障进行根本原因分析，会发现一个有多种伪装的单一罪魁祸首。

codeburst.io](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [](https://medium.com/dev-genius/code-smell-16-ripple-effect-c5a2db81f37e) [## 代码气味 16 —连锁反应

### 微小的变化会产生意想不到的问题。

medium.com](https://medium.com/dev-genius/code-smell-16-ripple-effect-c5a2db81f37e) 

让我们假设我们有一个现有的过程。

该系统应用各种算法来推导监督学习模型的[超参数](https://en.wikipedia.org/wiki/Hyperparameter_(machine_learning))。

提出了新的要求:

> 能够在生产中实时查看每个策略的性能数据。

## 解除系统耦合

让我们看看流程入口点:

…监督学习课程:

和调用的方法:

在生产系统的情况下，我们必须做的第一件事是识别它当前的覆盖范围。该系统有一系列自动化的单元和功能测试。

为了测量覆盖率，我们将使用[突变测试技术](https://en.wikipedia.org/wiki/Mutation_testing)。

不幸的是，只有一个测试失败了，所以我们发现这个过程没有包括在内，我们看到 Michael Feathers 的格言被可悲地应用:

> “继承的系统是没有测试的系统”

重构继承系统的策略是在做任何改变之前覆盖现有的功能。

## 1-创建延迟测试。

编写测试揭示了对象之间良好的设计接口。由于当前的解决方案及其包含的耦合，编写测试非常困难。

然而，如果没有事先编写测试，我们就不能重构来编写测试。似乎我们正面临一个恶性循环。

![](img/8e5b253853173841ba879efe1fd9789d.png)

[陈家](https://unsplash.com/@jkcphotos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/no-exit?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

这个死锁的可能解决方案是声明性地编写测试**，从而生成更好的接口。**

**我们将手动运行它们，直到耦合得到解决。**

## **2-我们编写测试来覆盖预先存在的功能。**

**可以用来自 xUnit 家族的工具编写带有错误断言的测试(它们总是失败)。**

**在涵盖了(目前是手动地)必要的案例之后，我们可以开始重构了。**

## **3-类名不代表双射中的真实名称。**

**帮助者不存在于现实世界中，也不应该存在于任何可计算的模型中。**

**让我们考虑一下在[映射器](https://codeburst.io/what-is-software-9a78c1172cf9)中选择名称的责任。**

**[](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

现在这个名字已经足够好了，它让我们了解了你的实例在现实世界中的职责。

## 4-该类是单例类。

没有正当的理由使用单例。这个事实，除了产生这里描述的所有问题之外:

[](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 辛格尔顿:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) 

产生一个非常易实现的调用(耦合到 *getInstance()* )而不是非常具有声明性...

我们将更改为:

留下如下的类定义:

一个重要的设计规则是:

> 不要继承具体的类。

如果语言允许这样做，我们显式声明它:

## 5-所有方法中的参数相同。

对象被创建，然后它获得一个神奇的参数，设置要优化的流程的标识符。这个论点通过各种方法传播。

这是一个*代码气味*建议我们检查该参数和流程之间的**内聚性**。

看一下双射，我们得出结论，没有过程就没有算法。我们不想让一个带有 *setters* 的类变异它:

[](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) [## 裸体模特第一部分:模特

### 旧的可靠数据结构及其有争议的(写)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) 

因此，我们将在建造过程中通过所有的**基本**属性。

知道一个属性是否**必要的**的方法是去掉与该对象相关的所有责任。如果它不能再执行它的职责，那是因为该属性属于[最小属性集](https://en.wikipedia.org/wiki/Maximal_and_minimal_elements)。

这样，这个战略在本质上是**不可改变的**，以及它带给我们的所有好处。

[](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) [## 变种人的邪恶力量

### 变异就是进化。它是由查尔斯·达尔文爵士提出的，我们在软件行业中使用它。但是有些事情是…

codeburst.io](https://codeburst.io/the-evil-powers-of-mutants-f803281ef82e) 

## 6 —我们找到一种设计模式。

根据双射理论，这个过程模拟了真实世界的过程。这似乎符合 [**命令**](https://en.wikipedia.org/wiki/Command_pattern) 的模式。

然而，我们认为它更接近于一个 [**方法对象**](https://refactoring.guru/replace-method-with-method-object) ，其中有一个有序的执行序列，对算法的不同步骤进行建模。

## 7-可互换行为类似于另一种模式。

正如我们根据其职责分配给对象的名称所暗示的那样，这个过程模拟了一个将与其他多态策略竞争的执行策略。

这就是[策略模式](https://en.wikipedia.org/wiki/Strategy_pattern)的意图。

名称应该与观察到的职责相匹配。

[](https://medium.com/dev-genius/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) [## 名字到底是什么？—第一部分:探索

### 我们都同意:好名声永远是最重要的。让我们找到他们。

medium.com](https://medium.com/dev-genius/what-exactly-is-a-name-part-i-the-quest-b812a4b1e0bf) ![](img/c7b758777cbd80bb7b4403d082b2381f.png)

尼古拉斯·霍伊泽在 [Unsplash](https://unsplash.com/s/photos/athletics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 8 —我们消除空值。

从来没有一个有效的理由使用空。 **Null** 在现实生活中是不存在的。

违反了**双射**的原理，在函数调用方和实参之间产生耦合。此外，它会生成不必要的**if**，因为 **null** 与任何其他对象都不是多态的。

我们将参数的缺失更改为布尔真值。

[](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0) [## Null:十亿美元的错误

### 他不是我们的朋友。它不会简化生活，也不会让我们更有效率。只是更懒。是时候停止了…

codeburst.io](https://codeburst.io/null-the-billion-dollar-mistake-c2918c92f7e0) 

## 9-我们删除默认参数。

上例中的私有函数有一个默认参数。

默认参数产生**耦合**和涟漪效应。它们可供懒惰的程序员使用。因为它是一个私有函数，所以替换作用域是同一个类。我们将其显式化，替换所有调用:

[](https://mcsee.medium.com/code-smell-19-optional-arguments-c0714855dbbb) [## 代码味道 19 —可选参数

### 伪装成友好的快捷方式是另一种耦合气味。

mcsee.medium.com](https://mcsee.medium.com/code-smell-19-optional-arguments-c0714855dbbb) 

## 10-我们移除硬编码的常数。

这些耦合在代码中的常量不允许我们“操纵时间”来做好测试。

请记住，测试必须**控制整个环境**，时间是**全局**和**脆弱**以匹配测试。

从现在开始，它将是对象创建的必要参数(通过[添加参数进行重构](https://refactoring.guru/es/add-parameter)是一项安全的任务，任何现代 IDE 都可以完成。

[](https://medium.com/@mcsee/code-smell-02-constants-and-magic-numbers-d6c320eef90b) [## 代码气味 02——常数和幻数

### 本文是 CodeSmell 系列的一部分。

medium.com](https://medium.com/@mcsee/code-smell-02-constants-and-magic-numbers-d6c320eef90b) 

## 11 —我们将日志解耦。

日志存储生产中有关策略执行的相关信息。像往常一样，使用一个*单例*作为全局引用。

这种新的结合使我们无法测试它。这个 *Singleton* 在另一个我们无法控制的模块中，所以我们将使用包装技术。

除了是单例的，日志还使用了静态的类消息。

让我们记住:

> 一个类应该包含的唯一协议是与其单一职责相关的协议(S 代表 Solid):创建实例。

因为引用的是静态方法，所以我们不能用多态方法替换类调用。相反，我们将使用匿名函数。

然后，我们可以通过减少耦合，从策略中产生更好的内聚性，并有利于它的可测试性，来分离对日志的引用，并从类中提取它。

我们现在可以将该对象与几种不同类型的记录器一起使用(比如 [tests doubles](https://en.wikipedia.org/wiki/Test_double) )。

通过生产代码的调用:

测试的结果是:

## 12 —我们具体化对象。

在我们重构的过程中，我们发现了一些对持久数据的修复。这种数据以内聚的方式传输，因此将它视为具有现实责任的对象是有意义的:

通过创造新的概念，我们处于建立一个贫血模型的危险之中。让我们看看你有哪些隐藏的责任:

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) 

## 13 —我们完成了覆盖范围。

我们没有忘记编写我们在开始时无法编写的测试程序。因为我们有一个耦合度更低的设计，所以现在很容易做到。

与我们发现它时相比，我们的系统更少“遗留”。

![](img/3593b74c480573a2bf2697973b025c15.png)

凯利·西克玛在 [Unsplash](https://unsplash.com/s/photos/tidy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 摘要

经过艰苦的迭代和增量工作，通过简短的步骤，我们在以下几个方面实现了更好的解决方案:

*   耦合更少。
*   不变性。
*   更好的名字。
*   没有 setter/getter。
*   没有如果。
*   没有 Null。
*   没有单身人士。
*   没有默认参数。
*   更好的测试覆盖率。
*   遵循开放/封闭原则(Solid 的 O ),能够添加新的多态算法。
*   遵循单一责任原则(S 为固体)。
*   而不用协议使类过载。

![](img/ff0a0ab2fb307dcdb0166cc6987d2a35.png)

由 [Zac 农民](https://unsplash.com/@zacfarmerart?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/very-old?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 结论

考虑到清晰的设计规则并采取小步骤，通过改进设计来修改现有系统是可能的。我们必须有专业的责任感和勇气来做出相关的改变，留下一个比我们找到它时好得多的解决方案。

我们甚至可以使用这些技术在遗留系统上进行 TDD。

[](https://mcsee.medium.com/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) [## 如何在遗留系统上挤压测试驱动开发

### 我们都喜欢 T.D.D .我们知道它的好处，我们已经阅读了一千篇关于如何使用它来构建系统的教程…

mcsee.medium.com](https://mcsee.medium.com/how-to-squeeze-test-driven-development-on-legacy-systems-3e66625c4fb1) 

这一系列文章的部分目标是为软件设计的辩论和讨论提供空间。

[](https://medium.com/@mcsee/object-design-checklist-47c63d351352) [## 目标设计清单

### 这是已经发表的软件设计文章的索引。

medium.com](https://medium.com/@mcsee/object-design-checklist-47c63d351352) 

我们期待着对这篇文章的评论和建议。

这篇文章还有西班牙语版本[这里](https://medium.com/dise%C3%B1o-de-software/c%C3%B3mo-desacoplar-un-sistema-heredado-e7fca5ffe628)。**