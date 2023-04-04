# 如何解决跳步问题

> 原文：<https://blog.devgenius.io/how-to-solve-skipping-steps-problem-20640b5631a6?source=collection_archive---------1----------------------->

## 第 39 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/55610b32fc14fe3203944968e82d3948.png)

Sahand Hoseini 在 [Unsplash](https://unsplash.com/t/experimental?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

*   出免费故事？这里是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-solve-skipping-steps-problem-20640b5631a6?source=friends_link&sk=9503d2e08454dd5a44838451bc0622ee)
*   100 天到 LinkedIn，雅虎，甲骨文

# 介绍

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 39 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从**这里**开始你的**准备**！

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战[重点是亚马逊和脸书的面试](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)。

# 新的一天，新的力量，新的想法

# 第 39 天——使用动态编程跳过步骤的问题🏁

# 目的

你在爬楼梯。要走 n 步才能到达顶端。

每次你可以爬 1 或 2 级台阶。有多少种不同的方式可以让你爬上顶峰？

# Example🕶

```
**Input:** 2
**Output:** 2
**Explanation:** There are two ways to climb to the top.
1\. 1 step + 1 step
2\. 2 steps
```

> 关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法

1.  这是一个**动态编程**的问题。如果可以从子问题的最优解中获得解，那么就必须考虑使用动态规划方法。
2.  大多数情况下，无论何时使用 dp 数组，都将使用值**(给定数组的长度+ 1)进行实例化。**
3.  结果将是 **dp 数组的最后一个元素。**
4.  这里，您必须用 1 和 2 实例化数组的前两个元素。因为这是获得第一名和第二名的最佳方式。
5.  现在对于 **dp 数组的每个索引。**如果使用 **dp 数组的前两个索引之和。**那么这个值无论如何都是最优解。
6.  在遍历结束时，返回 **dp 数组**的最后一个元素

# 边缘案例

1.  如果只有一步，返回一步。因为不需要使用额外的存储空间。

# 复杂性分析

> **时间复杂度:O(N)你遍历整个数组一次**
> **空间复杂度:O(N)你创建一个大小为 N 的 dp 数组**

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个极其有用的链表面试技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
[亚马逊 SDE 25 大面试问题](https://medium.com/javarevisited/top-25-amazon-sde-interview-questions-cfe0ef70ba9e?source=your_stories_page---------------------------)
[你以为你真的了解斐波那契数列吗？](https://medium.com/javarevisited/are-you-making-these-fibonacci-number-mistakes-5e3cbedd367e?source=your_stories_page---------------------------)
[用 C 编程解决 9 个最佳字符串问题](https://medium.com/@akshay_ravindran/9-best-strings-problem-solved-using-c-5e2a1d373fc2?source=your_stories_page---------------------------)
[一个人不简单地解决 50 个黑客等级挑战](https://medium.com/javarevisited/top-50-coding-challenges-in-hacker-rank-3d79c181528?source=your_stories_page---------------------------)

# 线的尽头

你现在已经到了这篇文章的结尾。谢谢你阅读它。祝你编程面试好运！

如果你在面试中遇到这些问题。请在下面的评论区分享它。我会很高兴读到它们。

[](https://medium.com/javarevisited/the-ultimate-guide-to-binary-trees-47112269e6fc) [## 二叉树的最终指南

### 任何你必须知道的关于二叉树的事情！

medium.com](https://medium.com/javarevisited/the-ultimate-guide-to-binary-trees-47112269e6fc) 

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 来接收更新。告诉我们你是如何**解决**这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)