# 敏捷的原子

> 原文：<https://blog.devgenius.io/the-atom-of-agile-ff0b3537643f?source=collection_archive---------12----------------------->

## 不使用子任务也可以划分用户故事

![](img/d060aba76a37d0c35d53fac2b6fd7610.png)

https://www . thoughtco . com/definition-of-atomic-weight-604378

## 什么是用户故事和任务？

在[软件开发](https://en.wikipedia.org/wiki/Software_development)和[产品管理](https://en.wikipedia.org/wiki/Product_management)中，**用户故事**是对软件系统的一个或多个特性的非正式的、自然的描述。用户故事通常是从系统的[终端用户](https://en.wikipedia.org/wiki/User_(computing)#End-user)或[用户的角度编写的。](https://en.wikipedia.org/wiki/User_(system))

任务就是任务。在敏捷中，通常是编码这个，设计那个，创建测试数据，验证测试假设等等。
 *-*-[*任务是关于*实现*；用户故事是关于*定义*的。*](https://medium.com/agile-it/composing-meaningful-tasks-c1ca51064c1a)

子任务:

*   子任务只是与用户故事相关的任务。
*   一旦所有子任务都完成了，用户故事也就完成了。
*   子任务对用户没有价值。
*   产品负责人对子任务不感兴趣。
*   子任务更容易估计。这通常是在优化会话中使用它们的主要原因。真的要估算吗？

## 子任务的问题

我要开始说，子任务没有问题，问题在于团队如何处理子任务。在我的经验中，团队在精化阶段的时候会将用户故事分割成子任务。

*   这是产品负责人和开发人员第一次一起谈论这些故事，所以在我看来，现在决定做什么还为时过早。这违背了“在最后负责任的时刻做出决定”的原则。
*   接受用户故事的开发人员在某种程度上受限于精化阶段创建的计划。

除了这个问题，另一个问题也来了。假设我们将一个用户故事分成两个子任务，每个子任务由不同的开发人员负责。他们都完成了各自的子任务，但是:

*   谁对用户故事负责？
*   我们是否有另一个集成两个部分的子任务，或者至少验证集成正在工作？。这个新的集成子任务是否被阻塞，直到其他任务完成？

开发人员的视角改变了，他们认为他们必须完成子任务。似乎另一个将会填补这些子任务集成的空白。子任务现在是我们与团队成员交流的方式。

## [康威定律](https://en.wikipedia.org/wiki/Conway%27s_law)

“任何设计系统(广义定义)的组织都将产生一个设计，其结构是该组织通信结构的副本。”

有时创建子任务是为了与不同领域(前端、后端、系统管理员、数据库管理员等)的专家交流。所以我们一次又一次地重复相同的架构，因为我们一次又一次地重复相同的通信模式。我只想说，如果您和您组织中的其他成员对该架构感到满意，这不是问题。

![](img/4a0fca876e4c763751d8c4ade14cae22.png)

[https://www . thoughtworks . com/es/insights/blog/applying-con ways-law-improve-your-software-development](https://www.thoughtworks.com/es/insights/blog/applying-conways-law-improve-your-software-development)

有了用户故事，我们就有机会停止这样做，让这些不同的专家密切合作。如果我们不开始创建一个 [T 型团队](https://medium.com/@warren2lynch/scrum-team-i-shaped-vs-t-shaped-people-569de6fa494e)，团队中有各种各样的专家是不够的，停止使用子任务是一个很好的起点。

大用户故事可能是一个问题，因为我们将无法显示开发过程中的进展。

## 大用户故事

这是在没有子任务的情况下工作的要点。当分包大用户故事时，我们是在冒险，我们是在说，我们接受整个故事将在生产或什么都没有。另一种方法是将用户故事分成一组小的用户故事。事实上，这总是我们必须要做的，小的用户故事意味着投入生产的风险更小。因此，强迫团队避免使用子任务将在产品负责人和开发人员之间产生对话，以定义如何将每个大的用户故事分成对用户有价值的小故事(这就是用户故事的定义)。
这个循环将最终产生相同大小的故事，因此没有必要再次估计( [NoEstimates](https://medium.com/@neil2killick/noestimates-part-1-doing-scrum-without-estimates-b42c4a453dc6) )。有很多拆分用户故事的技术，但我更喜欢其中的一个 [SPIDR](https://blogs.itemis.com/en/spidr-five-simple-techniques-for-a-perfectly-split-user-story) 。

我不是说你不必使用子任务，只是它们不应该是第一选择。