# 代码气味 29-设置/配置

> 原文：<https://blog.devgenius.io/code-smell-29-settings-configs-39188e5b8316?source=collection_archive---------1----------------------->

*在控制板中改变系统行为是客户的梦想。软件工程师的噩梦。*

![](img/46aae3ef4a2f9b4ba12e7279ea4d612c.png)

# 问题

*   重复代码
*   [If 污染](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484)
*   全局使用
*   [联轴器](https://codeburst.io/coupling-the-one-and-only-software-design-problem-869e293a9f04)
*   测试性和测试场景的爆炸。
*   复杂性

# 解决方法

1.  避免设置
2.  创建多态对象。外部注射。

# 示例代码

## 错误的

## 对吧

# 侦查

这是一种架构模式，因此应该通过设计策略来控制/避免。

# 例子

*   外部连接设置
*   用户设置
*   [特征切换](https://en.wikipedia.org/wiki/Feature_toggle)

# 例外

*   有时，我们将功能捆绑作为一种保障机制。这在遗留系统中是可以接受的。在 [CI/CD](https://en.wikipedia.org/wiki/CI/CD) 系统中，这些开关的寿命应该很短。
*   超级参数设置应由配置对象管理。

# 标签

*   全局

# 结论

设置运行时行为对于软件系统来说是很好的。

我们应该配置我们的对象，这样它们就可以以不同的方式运行，我们应该通过显式的行为对象以显式的方式来实现。

这样，我们的代码将更具声明性、清晰性和可测试性。这并不像添加 *IF 语句*那么简单。这种懒惰的开发人员给我们的系统带来了很多耦合和意想不到的问题。

> *一个具有 300 个布尔配置的系统有更多的测试组合(2^300)，比宇宙中的原子数量(10^80).*

# 也被称为

*   功能切换

[](https://jeromedane.medium.com/feature-flags-are-dangerous-88ef9d6c9f04) [## 功能标志很危险

### 功能标志很危险，有更好的方法来完成你正在尝试做的事情。

jeromedane.medium.com](https://jeromedane.medium.com/feature-flags-are-dangerous-88ef9d6c9f04) 

# 更多信息

[](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484) [## 如何永远摆脱恼人的假设

### 为什么我们学习编程的第一条指令应该是最后一条？

medium.com](https://medium.com/dev-genius/how-to-get-rid-of-annoying-ifs-forever-317033474484) 

> *简单是效率的灵魂。*

*奥斯汀·弗里曼*

[](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) [## 软件工程大报价

### 有时一个简短的想法会带来惊人的想法。

medium.com](https://medium.com/dev-genius/software-engineering-great-quotes-3af63cea6782) 

本文是*编码嗅觉*系列的一部分。

[](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c) [## 如何找到代码中的“臭”部分

### 代码闻起来很难闻。让我们看看如何改变香味。

medium.com](https://medium.com/dev-genius/how-to-find-the-stinky-parts-of-your-code-fa8df47fc39c)