# 数据结构和算法:常见的图形算法

> 原文：<https://blog.devgenius.io/data-structures-and-algorithms-common-graph-algorithms-a51a8025fe3b?source=collection_archive---------12----------------------->

## 了解这些图形算法可以帮助你通过技术面试

每个计算机科学的学生都必须学习数据结构和算法。然而，对于自学成才的开发人员来说，这并不是一个常见的话题。了解你的数据结构和算法对面试准备非常有帮助，甚至可能对你的工作有帮助，这取决于它是什么。

让我们来看看一些最常见的图算法以及它们是如何工作的。我们不会讨论算法的完整代码实现，而是算法中的伪代码和步骤。将有深入的代码教程附加。我们今天要看的四种图形算法是:

*   Dijkstra 算法
*   弗洛伊德-沃肖尔算法
*   普里姆算法
*   克鲁斯卡尔算法

## Dijkstra 算法

Dijkstra 算法是计算机科学中最著名的最短路径算法之一。找到最短路径需要一个简单的方法。据传，艾德加尔·迪克斯特拉(Edsgar Dijkstra)在与他当时的未婚妻进行咖啡约会时，在一张餐巾纸上想到了这个算法！

以下是 Dijkstra 最短路径算法的伪代码:

1.  选择一个开始节点
2.  跟踪“已访问”的节点
3.  检查每个节点，看它是否被访问过，以及它离起始节点有多远
4.  这样做，直到所有节点都被访问，或者我们发现一个节点不能被访问

一旦你遍历了一个图中的所有节点，Dijsktra 的算法就会得到一个从起始节点开始的距离列表。

关于 [Dijkstra 在 Python 中的算法](https://pythonalgos.com/dijkstras-algorithm-in-5-steps-with-python/)的深度教程。

## 弗洛伊德-沃肖尔算法

Floyd-Warshall 是另一种最短路径算法。不像 Dijkstra 的算法不是在 Floyd 和 Warshall 约会时在餐巾背面发明的。相反，该算法有一个组合名称，因为罗伯特·弗洛伊德和史蒂芬·沃舍尔在 1962 年。它也与 1956 年发表的克莱尼算法密切相关。

Floyd Warshall 算法是一种动态编程算法，这意味着我们要随着时间的推移来构建它。以下是弗洛伊德·沃肖尔算法的步骤。

1.  选择一个起始顶点
2.  计算顶点集之间的最短距离
3.  如果任何计算的距离小于当前记录的距离，则更换当前的距离
4.  对每个顶点重复步骤 2 到 3

Python 中 [Floyd Warshall 算法的深度教程](https://pythonalgos.com/graph-algorithms-floyd-warshall-in-python/)。

## 普里姆算法

与 Dijkstra 或 Floyd-Warshall 的算法不同，Prim 的算法构建了一个最小生成树(MST)。MST 是表示图中节点集之间最短路径的树。让我们看看如何用 Prim 的算法创建一个 MST。

1.  选择一个顶点
2.  遍历树，找到距离最近的顶点
3.  将新顶点添加到最小生成树并更新距离矩阵
4.  重复步骤 2 和 3，直到 MST 完全构建完成

Python 中 [Prim 的算法深度教程。](https://pythonalgos.com/graph-algorithms-prims-algorithm/)

## 克鲁斯卡尔算法

和 Prim 的算法一样，Kruskal 的算法是一种从图中构建最小生成树的方法。Kruskal 算法和 Prim 算法的主要区别在于，当添加到 MST 时，算法寻找什么。Kruskal 的算法从挑选最短的边开始，然后找到最短的连接边。

让我们来看看克鲁斯卡尔算法的伪代码。

1.  找到开始 MST 的最短边
2.  查找现有 MST 的最短附着边
3.  添加找到的边并更新距离矩阵
4.  重复步骤 2 和 3，直到最小生成树完成

Python 中关于[克鲁斯卡尔算法的深度教程](https://pythonalgos.com/graph-algorithms-kruskals-algorithm-in-python/)。

## 进一步阅读

*   [Python 中的嵌套列表](https://pythonalgos.com/nested-lists-in-python/)
*   建立你自己的人工智能文本摘要器
*   【2020 年和 2021 年的七项新自然语言处理技术
*   [为什么编程容易而软件工程难](/why-programming-is-easy-but-software-engineering-is-hard-90019fd78ed5)
*   [用 Python 从头开始编写神经网络代码](https://pythonalgos.com/create-a-neural-network-from-scratch-in-python-3/)

如果您觉得这很有帮助，请在 Twitter 或 LinkedIn 上与您的朋友分享！要想无限制地访问 Medium 上的信息宝库，今天就注册成为 Medium 会员吧！更多软件和技术行业的职业故事和技巧，记得关注我，[唐](https://medium.com/u/1c4e6640433f?source=post_page-----90019fd78ed5-----------------------------------)！