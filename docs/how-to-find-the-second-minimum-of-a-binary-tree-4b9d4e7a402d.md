# 如何求一棵二叉树的第二个最小值？

> 原文：<https://blog.devgenius.io/how-to-find-the-second-minimum-of-a-binary-tree-4b9d4e7a402d?source=collection_archive---------13----------------------->

## 第 14 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/0f4d46c9b93d7b0f2b617f70aebfa1f6.png)

照片由斯蒂芬妮·哈维在 [Unsplash](https://unsplash.com/s/photos/short?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

*   出于免费的故事？下面是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-find-the-second-minimum-of-a-binary-tree-4b9d4e7a402d?source=friends_link&sk=482592ad521c0da26963bba7867f7861)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 14 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但是我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，那是因为我已经完成了一项挑战，重点是这个系列中的亚马逊和脸书面试问题:

[](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) [## 亚马逊 100 天—第一天

### 亚马逊最常问的 100 个问题。代码和方法的解释。

medium.com](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) 

# 新的一天，新的力量，新的想法🚀

# 第 14 天—二叉树的第二个最小值🏁

# 目的🏹

给定一个由非负值节点组成的非空特殊二叉树，其中该树中的每个节点恰好有`two`或`zero`个子节点。如果该节点有两个子节点，则该节点的值是其两个子节点中较小的值。更正式的说法是，属性`root.val = min(root.left.val, root.right.val)`始终有效。

给定这样一棵二叉树，你需要输出由整棵树中所有节点的值组成的集合中第二个最小的值。

> 如果不存在这样的**第二最小值**，则输出-1。

# Example🕶

```
**Input:** 
    2
   / \
  2   5
     / \
    5   7

**Output:** 5
**Explanation:** The smallest value is 2, the second smallest value is 5.**Input:** 
    2
   / \
  2   2

**Output:** -1
**Explanation:** The smallest value is 2, but there isn't any second smallest value.
```

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法👨‍🎓

1.  创建一个哈希集。使用有序遍历遍历二叉树。
2.  在每个节点上，将该节点添加到哈希集中。
3.  将最小值初始化为根值。创建一个新的变量 res，它将存储第二个最小值，并将其初始化为最大值。
4.  对于哈希集中的每个元素。检查最小元素是否小于当前元素，以及当前元素是否小于第二个最小元素(res)。
5.  如果是，则将第二个最小元素更改为当前值。
6.  在散列集的末尾，返回 res(第二个最小元素)。

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个非常有用的面试链表技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
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

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 来接收更新。告诉我们你是如何解决这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费的**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)