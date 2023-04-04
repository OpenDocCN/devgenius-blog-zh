# 如何用龟兔法求重复数？

> 原文：<https://blog.devgenius.io/how-to-find-a-duplicate-number-using-tortoise-and-rabbit-method-8fb6e67c3190?source=collection_archive---------8----------------------->

第 13 天–100 天到 LinkedIn、雅虎、甲骨文

![](img/f3e2acee036e0645c774e3fb35163254.png)

*   出于免费的故事？下面是我的 [**好友链接。**](https://medium.com/@akshay_ravindran/day-10-binary-tree-zig-zag-traversal-2265eb879f63?sk=cfdacdbd13f72bb911ceca2b858d756d)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战赛第 100 天的第 13 天**。**

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

感谢你制作了这本 50🖤顶级电子书

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的**面试问题**对你的**职业** **成长**至关重要。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并从这些挑战中采用相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，那是因为我已经完成了一项挑战，重点是这个系列中的亚马逊和脸书面试问题:

[](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) [## 亚马逊 100 天—第一天

### 亚马逊最常问的 100 个问题。代码和方法的解释。

medium.com](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) 

# 新的一天，新的力量，新的想法🚀

# 第 13 天—找到重复的号码🏁

# 目的🏹

给定一个数组 *nums* **包含 *n* + 1** 个整数，其中每个整数都在 **1 和 *n* 之间(含)，则**证明至少有一个重复数存在。假设**只有一个重复的数字**，找出重复的那个。

1.  你**不能**修改数组(假设数组是只读的)。
2.  你必须只使用常量， *O* (1)额外空间。
3.  你的运行时复杂度应该小于 *O* ( *n* )。
4.  数组中只有一个重复的数字，但它可以重复多次。

# Example🕶

```
**Input:** [1,3,4,2,2]
**Output:** 2**Input:** [3,1,3,4,2]
**Output:** 3
```

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法👨‍🎓

1.  给出的最重要的提示是，数字的范围是从 **1 到 n。**然后我们可以使用两个指针的方法来解决这个问题。
2.  **数组**的**索引**和**数组**中的**值**将包含相同的数字。
3.  双指针方法由一个移动一个**步**的**慢指针**组成。移动**两步的**快速**指针。**
4.  因为数组中的所有值都可以被假定为索引，所以不会出现越界异常。我们认为数组中的值是索引。
5.  两个指针都从第一个元素开始。
6.  慢速指针通过移动到由当前值表示的索引来移动。
7.  快速指针以相同的方式移动两次。
8.  当他们相遇时，跳出这个圈子。
9.  现在，初始化指向数组开始的慢速指针。让快速指针在同样的位置。
10.  逐一移动**慢速**和**快速**指针**。**他们将在一个点相遇，这个点就是**重复号。**

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

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 以接收更新。告诉我们你**是如何解决**这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费的**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)