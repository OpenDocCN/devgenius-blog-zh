# 如何在旋转排序数组中搜索？

> 原文：<https://blog.devgenius.io/how-to-search-in-rotated-sorted-array-a68ed3fce98d?source=collection_archive---------8----------------------->

## 第 23 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/81ea3431f8c2823e72f0318b426074d8.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

*   出于免费的故事？下面是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-search-in-rotated-sorted-array-a68ed3fce98d?source=friends_link&sk=43a7b782c4aa52278efcebab152b60d8)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 23 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并从这些挑战中采用相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战[，重点是亚马逊和脸书的面试](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)。

# 新的一天，新的力量，新的想法🚀

# 第 23 天—在旋转排序数组中搜索🏁

# 目的🏹

假设一个按升序排序的数组在某个你事先不知道的支点上旋转。

(即`[0,1,2,4,5,6,7]`可能会变成`[4,5,6,7,0,1,2]`)。

您将获得一个要搜索的目标值。如果在数组中找到则返回其索引，否则返回`-1`。

您可以假设数组中不存在重复项。

您的算法的运行时复杂度必须在 *O* 的数量级(log *n*

# Example🕶

```
**Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4**Input:** nums = [4,5,6,7,0,1,2], target = 3
**Output:** -1
```

> 关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法👨‍🎓

1.  每当你遇到搜索+排序数组这个词。你可以用**二分搜索法解决它。**
2.  它以典型的二分搜索法方式开始，向左遍历比向右指针小。找到左右指针的中间，检查它是否是目标。
3.  如果是目标，返回它。如果不是，那么我们就这样修改算法。
4.  如果中间元素大于左侧元素，则左侧和中间元素之间的顺序是递增的。因此，如果目标小于中间值，则减少右索引，否则增加左索引。
5.  如果中间的元素小于左边的元素，则顺序被旋转。所以你必须比较中间的元素和右边的索引，然后检查目标是否大于中间，小于右边，向右移动，否则向左移动。
6.  当你找到目标时返回索引🔚

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