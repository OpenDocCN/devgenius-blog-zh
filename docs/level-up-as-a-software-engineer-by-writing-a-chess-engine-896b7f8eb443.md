# 通过编写国际象棋引擎来升级为软件工程师

> 原文：<https://blog.devgenius.io/level-up-as-a-software-engineer-by-writing-a-chess-engine-896b7f8eb443?source=collection_archive---------5----------------------->

## 第一部分

![](img/55e0180ea4e4813ef54e207be3ad7126.png)

比起阅读理论，我总是更擅长做项目。所以，当我开始学习 Golang 时，我想找一个需要各种软件工程概念的具有挑战性的项目。

我最终写了一个象棋引擎，它比我预期的更有挑战性。这也极大地提高了我应用重要软件工程概念的技能。我想分享让国际象棋编程成为提升软件工程师水平的绝佳途径的挑战。

我搭建的象棋引擎命名为 Glee，是 Golang 象棋引擎的简称。

*   [象棋引擎源代码](https://github.com/tonyOreglia/glee)
*   [前端源代码](https://github.com/tonyOreglia/personal-website/tree/master/src/ChessGame)
*   [在这里对抗欢乐合唱团！演示](https://tonycodes.com/chess)

象棋编程的以下方面将提高你的软件工程技能。

## 计算约束

一个象棋引擎的力量在某种程度上取决于它的搜索深度。例如，领先的开源国际象棋引擎 [Stockfish](https://stockfishchess.org/) ，平均搜索深度为 24。当然，搜索树是经过修剪的，所以并不是每一步都会被分析，但是为了让你对这里的规模有一些概念，让我们来检查一下数字。如果 Stockfish 只分析每个仓位的三次移动，那就是 3 个⁴仓位，即 282429536481 或 2820 亿。国际象棋引擎如何有效地确定合法的移动和评估位置优势对其实力至关重要。这对任何经验水平的开发人员来说都是一个有趣的、有价值的挑战。

## **算法**

一个给定的棋位平均有 31 个可能的走法。搜索深度为 24 的每个位置需要评估 6.2e+10 ⁵(或 620 百万分之一)个位置。这是不可能在国际象棋比赛的时间范围内计算的。有许多聪明的搜索算法、剪枝策略、散列方法和移动生成方法来减少搜索空间和提高效率。

仅仅作为一个例子，简单的 [Minimax](https://en.wikipedia.org/wiki/Minimax) 算法在给定一个搜索树并评估每个节点的情况下找到最优移动是有效的。然而，使用 [Alpha-beta 修剪](https://en.wikipedia.org/wiki/Alpha%E2%80%93beta_pruning)将通过忽略比先前分析的分支更差的分支来减少需要评估的节点数量。在最好的情况下，Alpha-beta 可以将搜索大小减少到原始搜索的平方根。

## 哈希表

理解何时以及如何使用哈希表在软件工程中至关重要。编写一个象棋引擎是提高这项技能的一个很好的机会。多次评估一个给定的位置显然是低效的，然而，随着游戏的进行，引擎当然会有当前和先前搜索树的大部分重叠。哈希表是一种通过保存职位的结果评估来节省时间和资源的有用方法。

## 链接列表

数据结构是计算机科学的核心。一个棋位和它的“子棋位”形成了一个有联系的可能性的树。构建具有高效插入、删除和搜索功能的数据结构是国际象棋比赛引擎的另一个关键部分。

## 成为计算历史的一部分

卡斯帕罗夫曾在 1997 年与 IBM 的深蓝国际象棋引擎交手。这是计算机在锦标赛条件下第一次击败国际象棋世界冠军。这是计算机技术进步的一个标志性时刻。今天，机器学习和人工智能越来越强大，无处不在。追随计算机科学先驱的脚步是一种很棒的体验。

## 性能测试和限制

> 对于工程师、设计师和其他创造性的问题解决者来说，正式定义他们必须在其中工作的约束对于引导能量和扩展创造力是至关重要的。
> 
> 亚当·摩根

在我看来，国际象棋编程最有益的方面就是表现是一等公民。每一次性能提升都将有利于引擎的棋谱评级。约束*应该*处于优秀工程的前沿，但是随着软件工程师可用的计算、内存和存储的不断增加，约束在大多数应用程序中已经变得越来越不重要。这很可悲，因为约束是工程学的核心，也是创造力的核心。我想起了一个鼓舞人心的故事，美国国家航空航天局(NASA)的工程师苦思如何将所有必要的牛顿方程塞进 4KB 的内存。象棋编程是一个将约束和性能问题带回你思考的前沿的机会。

![](img/76ba3f057f8da14a66d059b2e4025426.png)

我希望这个概述已经说服你继续探索国际象棋编程的世界。也许你甚至会编写自己的引擎。我确信这值得努力。

```
This article is part one of a series
- Find Part two about Data Structures [here](/improve-as-a-software-engineer-by-writing-a-chess-engine-c360109371aa).
- Find Part three about Move Generation [here](/level-up-as-a-software-engineer-by-writing-a-chess-engine-f4532f509b56).
- Find Part four about Testing [here](/level-up-as-a-software-engineer-by-writing-a-chess-engine-1d0ffc7aebf9).
```

如果你有兴趣和我一起深入国际象棋编程，[加入 Medium](https://tony-oreglia.medium.com/membership) ，[关注](https://tony-oreglia.medium.com/)，[订阅我的时事通讯](https://tony-oreglia.medium.com/subscribe)，或者在 [Twitter](https://twitter.com/tony_oreglia) 和 [LinkedIn](https://www.linkedin.com/in/tony-oreglia/) 上联系。

# 资源

*   [欢乐象棋引擎](https://github.com/tonyOreglia/glee)
*   [Glee React UI](https://github.com/tonyOreglia/personal-website/tree/master/src/ChessGame)
*   [欢乐合唱团试玩](https://tonycodes.com/chess)
*   [Stockfish](https://stockfishchess.org/)
*   此处讨论每回合平均合理移动范围内的可能游戏
*   在此讨论平均合法仓位