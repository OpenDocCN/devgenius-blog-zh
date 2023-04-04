# 如何使用动态规划找到油漆房子的最低成本？

> 原文：<https://blog.devgenius.io/how-to-find-minimum-cost-to-paint-a-house-in-java-b2ab250e3f63?source=collection_archive---------8----------------------->

## 第 18 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/da1a94ae32c10902067fe46fb2de3a29.png)

照片由[迪安娜从](https://www.pexels.com/@deeanacreates?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[像素](https://www.pexels.com/photo/close-up-photo-of-watercolor-palette-1576210/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)创造

*   出免费故事？下面是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-find-minimum-cost-to-paint-a-house-in-java-b2ab250e3f63?source=friends_link&sk=6989ddc8889206591c9c7c36f2c16448)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 18 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但是我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战，重点是 [**亚马逊和脸书面试问题**](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)

# 新的一天，新的力量，新的想法🚀

# 第 18 天——粉刷房子🏁

# 目的🏹

这里有一排房子，每栋房子都可以涂上三种颜色中的一种:红色、蓝色或绿色。每栋房子刷某种颜色的费用是不一样的。你必须油漆所有的房子，这样没有两个相邻的房子有相同的颜色。

用某种颜色粉刷每栋房子的成本由一个`*n* x *3*`成本矩阵表示。比如`costs[0][0]`是用颜色红色粉刷房子 0 的费用；`costs[1][2]`把房子 1 刷成绿色的成本，等等...找出粉刷所有房屋的最低成本。

# Example🕶

```
**Input:** [[17,2,17],[16,16,5],[14,3,19]]
**Output:** 10
**Explanation:** Paint house 0 into blue, paint house 1 into green, paint house 2 into blue. 
             Minimum cost: 2 + 5 + 3 = 10.
```

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法👨‍🎓

1.  这个问题是冒险进入**动态规划**【DP】的入门级别问题。
2.  你必须创建一个 dp 数组，它的**等于**成本矩阵的**长度**。****
3.  将 **dp 数组的第一个元素**初始化为 costs 中的第一个元素。
4.  对于每个矩阵，将特定索引的成本与在 **dp 数组中找到的两种不同颜色的最小成本**相加。
5.  对矩阵中的每个点**重复该步骤，直到到达数组的末尾。**
6.  在遍历结束时，返回在 dp 数组的最后一个元素中找到的最小成本。

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

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 以接收更新。告诉我们你是如何**解决**这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)