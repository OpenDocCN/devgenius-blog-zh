# 如何不用(+)运算符将两个数相加？

> 原文：<https://blog.devgenius.io/how-to-add-two-numbers-without-using-operator-356945cf8fa?source=collection_archive---------4----------------------->

## 第 32 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/e61af6eeff33b351e591febf2075cb31.png)

由[克里斯·贾维斯](https://unsplash.com/@crissyjarvis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/maths?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

*   出于免费的故事？下面是我的 [**好友链接。**](https://medium.com/@akshay_ravindran/day-10-binary-tree-zig-zag-traversal-2265eb879f63?sk=cfdacdbd13f72bb911ceca2b858d756d)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 32 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从**这里**开始你的**准备**！

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战[重点是亚马逊和脸书的面试](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)。

# 新的一天，新的力量，新的想法🚀

# 第 32 天—添加 2 个数字🏁

# 目的🏹

计算两个整数 *a* 和 *b* 的和，但是你**不允许**使用运算符`+`和`-`。

# Example🕶

```
**Input:** a = 1, b = 2
**Output:** 3
```

> 关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 边缘案例

1.  如果任何一个给定的数是零，返回另一个数。

# 算法👨‍🎓

![](img/d2ff570f2c2300dd5876b67bfe2e05f2.png)

[通过](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DthkTzdnkL5U&psig=AOvVaw0-QIC5KyOXKU8_7D7B8nNR&ust=1598311423864000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCLjP3YG9susCFQAAAAAdAAAAABAD)

1.  这种方法有点像我们用来将两个数相加的半加法器。
2.  和由两个输入的 **XOR** 表示，进位由两个输入的 and 表示。
3.  这里我们做同样的事情，因为我们必须为数字的每一位做，我们将进位向左移动到 1 位。
4.  对于每个位位置，我们对两个输入进行异或运算，这将是数组末尾的实际结果。🔚

# 复杂性分析

> **时间复杂度:O(1)**
> **空间复杂度:O(1)**

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个极其有用的面试链表技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
[亚马逊 SDE 面试前 25 题](https://medium.com/javarevisited/top-25-amazon-sde-interview-questions-cfe0ef70ba9e?source=your_stories_page---------------------------)
[你觉得你真的了解斐波那契数列吗？](https://medium.com/javarevisited/are-you-making-these-fibonacci-number-mistakes-5e3cbedd367e?source=your_stories_page---------------------------)
[用 C 编程解决 9 个最佳字符串问题](https://medium.com/@akshay_ravindran/9-best-strings-problem-solved-using-c-5e2a1d373fc2?source=your_stories_page---------------------------)
[一个人不简单地解决 50 个黑客等级挑战](https://medium.com/javarevisited/top-50-coding-challenges-in-hacker-rank-3d79c181528?source=your_stories_page---------------------------)

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