# 如何用 Java 求解 is 机器人在一个圆里？

> 原文：<https://blog.devgenius.io/how-to-solve-is-robot-in-a-circle-using-java-938afdfc48b5?source=collection_archive---------1----------------------->

## 第 52 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/5823fbf48cd9f20b132b5231165c6139.png)

照片由 [Alex Foret](https://unsplash.com/@createdby_alex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/t/wallpapers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

*   出于免费的故事？下面是我的 [**好友链接。**](https://akshay-ravindran.medium.com/938afdfc48b5?source=friends_link&sk=53b360b7d74501d898a5cf8715e5b974)
*   100 天到 LinkedIn，雅虎，甲骨文

# 介绍

嘿，伙计们，今天是领英挑战 100 天的第 52 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业生涯和成长都是至关重要的。从**这里**开始你的**准备**！

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战[，重点是亚马逊和脸书的面试](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)。

# 新的一天，新的力量，新的想法

# 第 52 天——机器人有界🏁

# 目的

在无限平面上，一个机器人最初站在`(0, 0)`处，面向北方。机器人可以接收三个指令中的一个:

*   `"G"`:直行 1 个单位；
*   `"L"`:向左转 90 度；
*   `"R"`:向右旋转 90 度。

机器人按顺序执行给定的`instructions`，并永远重复这些动作。

返回`true`当且仅当平面上存在一个圆，机器人永远不会离开这个圆。

# Example🕶

```
**Input:** "GGLLGG"
**Output:** true
**Explanation:** 
The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).
When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.
```

> 关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法

1.  创建一个存储机器人当前方向的字符串。初始化为北，因为它是起点。
2.  创建代表机器人坐标的 x，y。
3.  现在遍历给定的字符串。如果我们遇到 g .你必须根据机器人前进的方向增加或减少 x，y 坐标的值。
4.  如果你看到一个 L 或 R，那么你必须根据机器人当前所在的方向旋转机器人。
5.  如果两点都是 0，0 坐标，则返回 True，因为它又到达了起点一整圈。
6.  在遍历结束时，如果方向是北，则返回 false。因为它会无限移动。🔚

# 复杂性分析

> **时间复杂度:O(N)单次遍历给定字符串**
> 空间复杂度:O(1)不需要额外空间

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个极其有用的面试链表技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
[亚马逊 SDE 25 大面试问题](https://medium.com/javarevisited/top-25-amazon-sde-interview-questions-cfe0ef70ba9e?source=your_stories_page---------------------------)
[你认为你真的了解斐波那契数列吗？](https://medium.com/javarevisited/are-you-making-these-fibonacci-number-mistakes-5e3cbedd367e?source=your_stories_page---------------------------)
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