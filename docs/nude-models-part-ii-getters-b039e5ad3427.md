# 裸体模特第二部分:吸气剂

> 原文：<https://blog.devgenius.io/nude-models-part-ii-getters-b039e5ad3427?source=collection_archive---------3----------------------->

## 可靠的数据结构及其有争议的(读)访问。

使用对象作为数据结构是一种已确立的实践，它产生了许多与软件的可维护性和发展相关的问题。它误用了 50 年前提出的杰出概念。在这第二部分我们将反思对 ***阅读*** *这些对象的访问。*

在本文的第一部分中，我们展示了从数据结构中的隐藏信息到活动对象职责(本质的**什么**)隐藏实现(偶然的**如何**)的转换。

[](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) [## 裸体模特第一部分:模特

### 旧的可靠数据结构及其有争议的(写)访问。

medium.com](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3) 

在第二部分中，我们将展示使用 *getters* 的缺点。

![](img/6d573769821198e360beeab658509352.png)

照片由[张秀坤·万尼](https://unsplash.com/@dominik_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/mining?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 现实世界中不存在的名字(重复)

程序员习惯上使用***get attribute……()***形式的名称来公开(并失去对)一个先前私有的属性。由于 setter 的[文章](https://medium.com/dise%C3%B1o-de-software/information-showing-chapter-i-setters-138deb558e5d)中陈述的相同论点，这个名字不能通过[](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)**双射 [**映射**](https://codeburst.io/what-is-software-9a78c1172cf9) 到现实世界中的对等物。**

**关于这些名字的最终结论是:**

> **不应有 setAttribute…()或 getAttribute…()形式的方法**

# **不要公开集合**

**许多对象管理集合。内容管理、不变量或遍历方法应该是这些对象的唯一职责。**

**假设我们想在画布上绘制第一部分中呈现的多边形[。我们将用下面的代码来实现它:](https://medium.com/dev-genius/nude-models-part-i-setters-77ac784a91f3)**

**绘制多边形**

**通过暴露顶点集合(因为在大多数语言中集合是通过引用传递的[),我们失去了对集合的控制。](https://en.wikipedia.org/wiki/Evaluation_strategy#Call_by_reference)**

**没有什么可以阻止其他代码运行:**

**array_shift()从数组中移除第一个值**

**这导致三角形变异，产生不一致的真实世界双射。双面多边形会违反闭合图形的原则。**

**这个缺陷会在很久以后才被注意到，因为没有被及时发现，因此违反了 fail fast 原则。**

**[](https://codeburst.io/fail-fast-3f3f036032b0) [## 快速失败

### 失败是时尚。制造比思考容易得多，失败不是耻辱，让我们把这个想法带到我们的…

codeburst.io](https://codeburst.io/fail-fast-3f3f036032b0) 

> 在任何情况下，这些物品都不应该暴露它们的收藏，因此执行[得墨忒耳定律。](https://en.wikipedia.org/wiki/Law_of_Demeter)

万一需要归还收集到的元素，要用副本([浅](https://en.wikipedia.org/wiki/Object_copying))来回答，以免失控。以目前的技术水平，复制收藏品的速度非常快。如果它们是非常大的集合，那么可以使用迭代器、代理和游标来设计解决方案，以避免执行完全复制操作。

## 迭代集合

我们如何解决多边形绘制操作？

在使用设计模式时，迭代集合是一个众所周知的主题:

[](https://en.wikipedia.org/wiki/Iterator_pattern) [## 迭代器模式

### 在面向对象编程中，迭代器模式是一种设计模式，其中迭代器用于遍历一个

en.wikipedia.org](https://en.wikipedia.org/wiki/Iterator_pattern) 

如果我们想要遍历我们的多边形，我们可以返回一个迭代器(指示**我们需要做什么**而不暴露我们的底层数据结构(**如何**遍历它)。

返回迭代器允许对象改变它的表示

在语言支持[匿名函数](https://en.wikipedia.org/wiki/Anonymous_function)或[闭包](https://en.wikipedia.org/wiki/Closure_(computer_programming))的情况下，我们可以承担迭代元素的责任，而无需向外暴露迭代器:

## 变异集合

多边形不能变异，因为顶点是它们的最小本质**的一部分**:如果我们移除它们的任何顶点，它们就不再是使它们**独特的那个多边形**。

有许多业务对象可能在其意外集合中发生变异，并且有管理这种变异的机制。

如果我们想要建立一个 *Twitter* 账户并保留它的追随者，了解商业规则，那么这个账户是在没有追随者的情况下创建的。

在创建新账户时，让我们忽略它给我们的建议。

使用*设置器*和*获取器*，一个初学编程的人可能会想以这种方式添加一个追随者:

由业务规则指导的正确的责任分配表明，添加新的关注者、执行验证(例如，它以前没有被关注)以及保持集合完整性是帐户的责任。

因此，更好的解决方案是:

## 双重封装

在 90 年代，有一种趋势是创建属性的双重封装作为隐私的极端方法。这意味着，即使是从一个对象的私有方法，也要避免直接访问变量。

这种做法不会产生任何好处。添加不必要的间接寻址，并在没有区分**公共**和**私有**方法的语言中公开 setters 和 getters。

此外，它隐藏了属性和引用它的直接方法之间的**耦合**，避免了可能的重构。

[](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) [## 耦合:唯一的软件设计问题

### 对我们软件的所有故障进行根本原因分析，会发现一个有多种伪装的单一罪魁祸首。

codeburst.io](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04) 

## 告诉，不要问

有一条[原则](https://www.infoworld.com/article/2073723/why-getter-and-setter-methods-are-evil.html)规定:

> 不要询问你完成工作所需的信息；请拥有信息的对象为您工作

这个原理被称为:[告诉，不要问](https://martinfowler.com/bliki/TellDontAsk.html)。

它提醒我们，我们应该告诉一个对象**做什么**，而不是从一个对象那里请求**数据**并对这些数据采取行动。这鼓励了对象负责管理的**行为**和**知识**的移动。

## 太多的信息会害死我们

解释德米特里定律和最小耦合和最大内聚力定律:

*   每个单元对其他单元的了解应该是有限的，只知道那些与自己密切相关的单元。
*   每个单位应该只和自己的朋友说话，不要和陌生人说话。
*   只要和你最亲近的朋友谈谈。

用*设置器*和*获取器*添加**偶然**复杂性意味着产生耦合，违反这些规则，并在可能发生变化的情况下产生更大的涟漪效应。

![](img/86e1304a4ac82bc58d4628795870bdaf.png)

[澳门图片社](https://unsplash.com/@macauphotoagency?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/crime-scene?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## Setters 和 getters 违反了拟人论

让我们回到我们唯一的设计规则，它要求我们正在构建的模型和现实世界之间有一个[双投影](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)，并尊重[拟人化](https://en.wikipedia.org/wiki/Anthropomorphism)的原则(给每个对象一个活的实体)。

[](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) [## 唯一的软件设计原则

### 如果我们在一个单一的规则上建立我们的整个范式，我们可以保持它的简单并做出优秀的模型。

codeburst.io](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af) 

在这样做的时候，我们会发现在对象被返回一个 *getter* 后，我们赋予对象的**职责**并没有**映射**与现实世界中违反[双映射](https://codeburst.io/the-one-and-only-software-design-principle-5328420712af)。

在这个[页面上](https://www.yegor256.com/2014/09/16/getters-and-setters-are-evil.html#a-ball-and-a-dog)有一个使用 *getters* 时不尊重拟人化的极好例子。

## 改变我们的思维方式

当我们开始对忘记了它们的**偶然**表示的对象建模时，我们将能够避免[贫血类](https://en.wikipedia.org/wiki/Anemic_domain_model)(它们只完成保存数据的功能，导致了众所周知的[反模式](https://en.wikipedia.org/wiki/Anti-pattern))。

与数据结构一样，一个贫血的类无法保证数据和关系的完整性。

由于贫血类的操作在贫血类边界的之外**，因此没有**单点控制**。因此，我们将生成**重复代码**和存在于我们模型中的这些属性的访问点。我们将永远追求模仿像**黑盒**这样的对象的行为，得到更加真实和声明性的双射。**

[](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) [## 代码气味 01——贫血模型

### 你的对象是一堆没有行为的公共属性。

medium.com](https://medium.com/dev-genius/code-smell-01-anemic-models-f9fb5a1323b3) 

# 推荐

*   不要使用*设定器*。这样做没有充分的理由。
*   不要使用*吸气剂*。如果一个对象的任何职责与响应一个匹配属性的消息有关，那么如果我们没有**破坏封装，就要事先考虑好。**
*   不要在函数名前面加上 get 这个词。如果现实世界中的一个多边形能回答它的顶点是什么，那就用现实世界的名字(***)vertices()***)。
*   在返回集合的情况下，返回一个**副本**或一个**代理**，以免失去控制，有利于迭代器的使用。
*   没有**公共**属性。实际上，它就像有*设置器*和*获取器*。这也是一个*码闻*的要死不活的对象。
*   没有**公共静态**属性。除了上面列出的之外，类应该是无状态的，这是一个代码气味，表明一个类正被用作全局变量。

[](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) [## 辛格尔顿:万恶之源

### 允许的全局变量和假定的内存节省

codeburst.io](https://codeburst.io/singleton-the-root-of-all-evil-8e59ca966243) 

## 从传统代码系统过渡

并非都是坏消息。通过正确的**职责重新分配，**以及适当重构的帮助，将一个坏模型转换成一个好模型是可能的。

如果我们有机会用**良好的覆盖率**来改进系统，我们可以逐步封装对象，以一种**增量迭代的方式限制它们的访问。**

在没有足够覆盖的情况下，我们将面对一个[遗留代码系统](https://en.wikipedia.org/wiki/Legacy_code)，根据 [Michael Feathers](https://twitter.com/mfeathers) 的出色定义:

> 遗留代码系统是没有覆盖的系统。

如果是这种情况，我们必须**首先覆盖**现有的功能，然后我们可以执行必要的转换。

![](img/43fb95820c36477a6a42be8a07dfb185.png)

照片由[格雷格·努内斯](https://unsplash.com/@greg_nunes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/rainbow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 结论

使用*设置器*和*获取器*的良好惯例产生**耦合**并防止我们的计算机系统的**增量进化**。

根据这篇文章中陈述的论点，我们应该尽可能地限制它们的使用。

这一系列文章的部分目标是为软件设计的辩论和讨论提供空间。

[](https://medium.com/@mcsee/object-design-checklist-47c63d351352) [## 目标设计清单

### 这是已经发表的软件设计文章的索引。

medium.com](https://medium.com/@mcsee/object-design-checklist-47c63d351352) 

我们期待着对这篇文章的评论和建议。

这篇文章还有西班牙语版本[点击这里](https://medium.com/dise%C3%B1o-de-software/modelos-desnudos-parte-ii-getters-8432b8c6e4f3)。**