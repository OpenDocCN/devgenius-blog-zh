# 如何使用二分搜索法找到元素的第一个和最后一个位置？

> 原文：<https://blog.devgenius.io/how-to-find-the-first-and-last-position-of-element-using-binary-search-e9cf6e0d694c?source=collection_archive---------17----------------------->

## 第 20 天— 100 天去 LinkedIn、雅虎、甲骨文

![](img/b79e928bb52e169aaa8b1a404bb07967.png)

*   出于免费的故事？这里是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-find-the-first-and-last-position-of-element-using-binary-search-e9cf6e0d694c?source=friends_link&sk=6198cc9281bcc2b83bf1f3fcf515a9b4)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 20 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从**这里**开始你的**准备**！

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但是我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，那是因为我已经完成了一项挑战，重点是这个系列中的亚马逊和脸书面试问题:

[](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) [## 亚马逊 100 天—第一天

### 亚马逊最常问的 100 个问题。代码和方法的解释。

medium.com](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) 

# 新的一天，新的力量，新的想法🚀

# 第 20 天—元素的第一个和最后一个位置🏁

# 目的🏹

给定一个按升序排序的整数数组`nums`，找出给定`target`值的开始和结束位置。**你的算法的运行时复杂度必须在 *O* 的顺序(log *n* )。**

如果在数组中没有找到目标，返回`[-1, -1]`。

# Example🕶

```
**Input:** nums = [5,7,7,8,8,10], target = 8
**Output:** [3,4]**Input:** nums = [5,7,7,8,8,10], target = 6
**Output:** [-1,-1]
```

> 关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法👨‍🎓

1.  每当我们在搜索方面遇到问题，而你必须降低时间复杂度时，这几乎总是一个二分搜索法问题。
2.  二分搜索法的前提条件是数组必须排序。这里给定的数组已经是升序排列了，所以你不用担心。
3.  你将从两个指针开始，一个在数组的开头，另一个在数组的结尾。
4.  现在，你会发现数组的中间。如果中间的元素大于你要搜索的数字，我们必须向数组的左半部分移动，通过改变高位指针，
5.  如果中间的元素比目标元素小，你必须向数组的右半部分搜索，这可以通过改变低值来实现。
6.  我们添加到二分搜索法的一个变化是，我们总是返回低索引的位置，而不是返回目标元素的位置。这是为了确保边缘情况。
7.  现在要找到第一个和最后一个位置，您必须像平常一样搜索第一个元素。
8.  仅当您搜索第二个元素，而不是搜索第一个元素时，才会发生变化。您必须搜索一个大于目标元素本身的数字。
9.  初始化，位置为-1，-1 如果搜索后位置保持不变，则该元素不存在于数组中。
10.  否则返回索引。🔚

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个极其有用的面试链表小技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
[亚马逊 SDE 25 大面试问题](https://medium.com/javarevisited/top-25-amazon-sde-interview-questions-cfe0ef70ba9e?source=your_stories_page---------------------------)
[你以为你真的了解斐波那契数列吗？](https://medium.com/javarevisited/are-you-making-these-fibonacci-number-mistakes-5e3cbedd367e?source=your_stories_page---------------------------)
[用 C 编程解决 9 个最佳字符串问题](https://medium.com/@akshay_ravindran/9-best-strings-problem-solved-using-c-5e2a1d373fc2?source=your_stories_page---------------------------)
[一个人不能简单解决 50 个黑客等级挑战](https://medium.com/javarevisited/top-50-coding-challenges-in-hacker-rank-3d79c181528?source=your_stories_page---------------------------)

# 线的尽头

你现在已经到了这篇文章的结尾。谢谢你阅读它。祝你编程面试好运！

如果你在面试中遇到这些问题。请在下面的评论区分享它。我会很高兴读到它们。

[](https://medium.com/javarevisited/the-ultimate-guide-to-binary-trees-47112269e6fc) [## 二叉树的最终指南

### 任何你必须知道的关于二叉树的事情！

medium.com](https://medium.com/javarevisited/the-ultimate-guide-to-binary-trees-47112269e6fc) 

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 接收更新。告诉我们你是如何解决这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)