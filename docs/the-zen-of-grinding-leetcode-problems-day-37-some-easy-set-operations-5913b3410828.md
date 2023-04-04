# 破解 LeetCode 问题的禅:第 37 天——一些简单的集合运算

> 原文：<https://blog.devgenius.io/the-zen-of-grinding-leetcode-problems-day-37-some-easy-set-operations-5913b3410828?source=collection_archive---------15----------------------->

欢迎回到 [**LeetCode 日常练习系列**](https://medium.com/@matei.danut.dm/the-zen-of-grinding-leetcode-problems-day-0-motivation-681842565166) **。**今天我们重点关注 **3 易**套题。开始吧！

![](img/3b525e477f30eb7ec7a3e0755aa2c567.png)

# 两个数组的交集

[](https://leetcode.com/problems/find-the-difference-of-two-arrays/submissions/) [## 求两个数组的差- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/find-the-difference-of-two-arrays/submissions/) 

**见解**:

*   请记住，Python 有一个针对集合的**专用集合，所以您能想到的大多数集合操作都已经实现了(不幸的是，**多重集合**还没有实现)**
*   在大多数应用程序中，在应用集合操作后，您需要将**转换回列表**。

# 多个数组的交集

[](https://leetcode.com/problems/intersection-of-multiple-arrays/) [## 多个数组的交集- LeetCode

### 给定一个 2D 整数数组 nums，其中 nums[i]是不同正整数的非空数组，返回…

leetcode.com](https://leetcode.com/problems/intersection-of-multiple-arrays/) 

**见解**:

*   在某些应用程序中，重要的一点是集合中元素的顺序。不幸的是，Python **不保证任何顺序**，为了保证操作的效率。
*   因此，您需要在最后对结果列表进行排序

# 求两个数组的差

[](https://leetcode.com/problems/find-the-difference-of-two-arrays/) [## 求两个数组的差- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/find-the-difference-of-two-arrays/) 

**见解**:

*   对于集合，**-运算符重载**以自动表示集合差。所以你不需要记住任何其他的方法名。
*   不幸的是，**+操作符并没有重载来表示集合并。**

结束语:

*   今天我差点忘了写文章。我刷牙的时候想起了这件事，我真的打算今晚睡个好觉，但是哦好吧。
*   所以我的目标是解决简单的问题，并完成它。最后，我做了 3 个，但都不太耗时。代码很少，课程很短，也许你从中学到了新的东西，也许没有。
*   但是想象一下，如果我给自己一个不同的承诺:每周发表一篇精心研究的文章。如果我知道离最后期限还有 5 天的话，我根本不可能做任何工作并减少我的睡眠时间！即使额外 1 小时可以显著提高内容的最终质量。
*   当你还没有*深入一个项目*的时候，你很难看到**这种小规模决策**的影响。我的意思是，假设你**估计一项任务将在大约 100 个小时**的工作中完成，假设这项任务*从一个草稿到下一个*变得更好。如果你只投资**80 小时**，最终结果会差多少？那 **60 小时**呢？
*   事实是，我们真的不能很好地估计做一件事需要做多少工作。要创造出 ***真正价值*** 的东西(比如你可以出版的一本技术书籍)，将会花费**两倍的时间**，最终结果将会是**比你想象的**好一半**。**
*   然而，你仍然应该发表它，因为如果你把它作为草稿，对任何人都没有帮助。增加的经验会对你的下一个项目有所帮助。