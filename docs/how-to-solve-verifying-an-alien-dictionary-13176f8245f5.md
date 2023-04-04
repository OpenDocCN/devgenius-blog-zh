# 如何解决验证一本外星人字典？

> 原文：<https://blog.devgenius.io/how-to-solve-verifying-an-alien-dictionary-13176f8245f5?source=collection_archive---------1----------------------->

## 第 38 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/a9f46b8a71603485f60bdf3a2dfb4a7f.png)

照片由 [Ravi Pinisetti](https://unsplash.com/@ravipinisetti?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/calm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*   出于免费的故事？下面是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-solve-verifying-an-alien-dictionary-13176f8245f5?source=friends_link&sk=858f6669dbc8119abe46eb2f7dc4dbe1)
*   100 天到 LinkedIn，雅虎，甲骨文

# 介绍

嘿，伙计们，今天是领英挑战 100 天的第 38 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业生涯和成长都是至关重要的。从**这里**开始你的**准备**！

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战[，重点是亚马逊和脸书的面试](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)。

# 新的一天，新的力量，新的想法

# 第 38 天——验证一本外星人字典🏁

# 目的

在外星语言中，令人惊讶的是他们也使用英文小写字母，但可能是不同的`order`。字母表中的`order`是一些小写字母的排列。

给定一个用外星语言写的`words`序列和字母表的`order`，当且仅当给定的`words`在这种外星语言中按字典顺序排序时，返回`true`。

# Example🕶

```
**Input:** words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
**Output:** false
**Explanation:** As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.
```

> 关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法

1.  将订单存储在散列表中，用**键**作为**字符**，用值作为从 **0 到 25 的索引。**
2.  现在我们知道这些字符中的哪些出现在哪些索引中。
3.  遍历单词列表，检查每个相邻单词的顺序是否正确。
4.  如果每一对的顺序都正确。那么所有对的顺序都是正确的。
5.  对于每一对，检查按顺序排列的字符的索引值是否小于第二个。如果是，则返回 true。
6.  如果第一个单词有更高阶的值，则返回 false。

# 边缘案例

1.  边缘情况是如果两个单词具有相同的**词干**。那么它可能是真或假取决于每个单词的大小。
2.  赢了纳闷-> **假了**
3.  赢了赢了-> **真**

# 复杂性分析

> **时间复杂度:O(n)遍历整个单词列表**
> **空间复杂度:O(1)使用一个常量空间 hashmap**

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个极其有用的面试链表技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
[亚马逊 SDE 25 大面试问题](https://medium.com/javarevisited/top-25-amazon-sde-interview-questions-cfe0ef70ba9e?source=your_stories_page---------------------------)
[你觉得你真的了解斐波那契数列吗？](https://medium.com/javarevisited/are-you-making-these-fibonacci-number-mistakes-5e3cbedd367e?source=your_stories_page---------------------------)
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