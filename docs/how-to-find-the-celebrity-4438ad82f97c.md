# 如何找到名人？

> 原文：<https://blog.devgenius.io/how-to-find-the-celebrity-4438ad82f97c?source=collection_archive---------25----------------------->

## 第 21 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/8a701c9dc5f9d384a453eef93cc75537.png)

*   出于免费的故事？下面是我的 [**好友链接。**](https://medium.com/@akshay_ravindran/4438ad82f97c?source=friends_link&sk=21f279eabf1e375692a73dd3fb0c0ce2)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 21 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但是我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战[，重点是亚马逊和脸书的面试](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)。

# 新的一天，新的力量，新的想法🚀

# 第 21 天——找到名人🏁

# 目的🏹

假设你和`n`个人(从`0`到`n - 1`)在一个聚会上，其中可能有一个名人。名人的定义是所有其他人都认识他/她，但他/她不认识他/她。

现在你想找出谁是名人，或者确认没有名人。你唯一被允许做的事情是问这样的问题:“嗨，a。你认识 B 吗？”为了获得 A 是否认识 b 的信息，你需要通过问尽可能少的问题(在渐近意义上)来找出名人(或者核实没有名人)。

给你一个帮助函数`bool knows(a, b)`，它告诉你 A 是否知道 b。实现一个函数`int findCelebrity(n)`。如果他/她在派对上，那就只有一个名人。如果聚会中有名人，则返回名人的标签。如果没有名人，返回`-1`。

# Example🕶

![](img/61c7d2fdb39bc5980b749c440e1b9f32.png)

```
**Input:** graph = [
  [1,1,0],
  [0,1,0],
  [1,1,1]
]
**Output:** 1
**Explanation:** There are three persons labeled with 0, 1 and 2\. graph[i][j] = 1 means person i knows person j, otherwise graph[i][j] = 0 means person i does not know person j. The celebrity is the person labeled as 1 because both 0 and 2 know him but 1 does not know anybody.
```

> 关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法👨‍🎓

1.  你必须根据这个条件找到一个可能是名人的候选人，如果 **A 认识 B** ，那么 **B** 可能**成为名人的**候选人**，因为我们既然 **A 认识 B****A 就不能**成为**名人**。**
2.  **遍历**数组中的所有元素来寻找一个候选者，现在在你找到一个候选者之后。
3.  再一次比较这个**候选人**将数组的所有其他成员，找出这个候选人是否是名人。
4.  如果候选人认识某人或某人不认识候选人，这可以用条件来确定。那么候选人是**不是名人。**
5.  否则还名人。🔚

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个极其有用的面试链表技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
[亚马逊 SDE 25 大面试问题](https://medium.com/javarevisited/top-25-amazon-sde-interview-questions-cfe0ef70ba9e?source=your_stories_page---------------------------)
[你认为你真的了解斐波那契数列吗？](https://medium.com/javarevisited/are-you-making-these-fibonacci-number-mistakes-5e3cbedd367e?source=your_stories_page---------------------------)
[用 C 编程解决 9 个最佳字符串问题](https://medium.com/@akshay_ravindran/9-best-strings-problem-solved-using-c-5e2a1d373fc2?source=your_stories_page---------------------------)
[一个人不能简单解决 50 个黑客等级挑战](https://medium.com/javarevisited/top-50-coding-challenges-in-hacker-rank-3d79c181528?source=your_stories_page---------------------------)

# 线的尽头

你现在已经到了这篇文章的结尾。谢谢你阅读它。祝你编程面试好运！

如果你在面试中遇到这些问题。请在下面的评论区分享它。我会很高兴读到它们。

[](https://medium.com/javarevisited/the-ultimate-guide-to-binary-trees-47112269e6fc) [## 二叉树的最终指南

### 任何你必须知道的关于二叉树的事情！

medium.com](https://medium.com/javarevisited/the-ultimate-guide-to-binary-trees-47112269e6fc) 

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 以接收更新。告诉我们你**是如何解决**这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费的**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)