# 如何将一个列表旋转 K 次？

> 原文：<https://blog.devgenius.io/how-to-rotate-a-list-k-times-7c4ebaed4ab7?source=collection_archive---------25----------------------->

## 第 16 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/fa7703ae8be02a909962ea02b2674adf.png)

*   出于免费的故事？下面是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-rotate-a-list-k-times-7c4ebaed4ab7?source=friends_link&sk=ce925335800cb50908ad1f63e9fce787)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战赛第 100 天的第 16 天**。**

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的**面试问题**对你的**职业** **成长**至关重要。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并从这些挑战中采用相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，那是因为我已经完成了一项挑战，重点是这个系列中的亚马逊和脸书面试问题:

[](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) [## 亚马逊 100 天—第一天

### 亚马逊最常问的 100 个问题。代码和方法的解释。

medium.com](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) 

# 新的一天，新的力量，新的想法🚀

# 第 16 天—轮换名单🏁

# 目的🏹

给定一个链表，将链表向右旋转 *k* 位，其中 *k* 为非负。

# Example🕶

```
**Input:** 1->2->3->4->5->NULL, k = 2
**Output:** 4->5->1->2->3->NULL
**Explanation:**
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

> 每当旋转出现问题时，如果 K 大于列表中的值，一定要记住减少循环次数。

# 算法👨‍🎓

1.  如果知道一个链表的三个节点，**尾节点**，第**个节点**，第**个节点之后的节点，就可以解决这个问题。**
2.  遍历列表，直到到达列表的末尾，将尾部节点存储在一个变量中(end ),同时在遍历时计算列表中元素的数量。
3.  现在遍历到第 k 个节点，将第 k 个和第 k _ next 节点存储在不同的变量中。
4.  现在，将**最后一个节点**连接到**第 k 个节点。**
5.  使**第 k 个节点**指向**空，**使**第 k 个节点**成为列表的**结尾。**
6.  将列表的**头**改为**第 k _ next 节点。**

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

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费的**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)