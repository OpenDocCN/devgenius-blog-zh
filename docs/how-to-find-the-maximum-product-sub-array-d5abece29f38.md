# 如何求最大乘积子阵列？

> 原文：<https://blog.devgenius.io/how-to-find-the-maximum-product-sub-array-d5abece29f38?source=collection_archive---------15----------------------->

## 第 17 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/163fd2a90ea34eae6b8256ae8f2121f1.png)

来自 [Pexels](https://www.pexels.com/photo/close-up-photo-of-lip-gloss-3373741/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[闪亮钻石](https://www.pexels.com/@shiny-diamond?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的照片

*   出于免费的故事？下面是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-find-the-maximum-product-sub-array-d5abece29f38?source=friends_link&sk=65b2cce279edcc9e4f4f0fa1789585a2)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 17 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道为什么我选择了 LinkedIn、Yahoo 和 Oracl e 而不是 FAANG，那是因为我完成了一项挑战 [**重点是亚马逊和脸书的面试问题**](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)

# 新的一天，新的力量，新的想法🚀

# 第 17 天—最大产品子阵列🏁

# 目的🏹

给定一个整数数组`nums`，在一个数组(至少包含一个数)中找出具有最大乘积的连续子数组。

# Example🕶

```
**Input:** [2,3,-2,4]
**Output:** 6
**Explanation:** [2,3] has the largest product 6.**Input:** [-2,0,-1]
**Output:** 0
**Explanation:** The result cannot be 2, because [-2,-1] is not a subarray.
```

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

你应该知道如何在一个子数组中找到**最大和**，这已经成为常识。如果没有，我希望你在解决这个问题之前先看一下这个 [**点击这里**](https://medium.com/@akshay_ravindran/100-days-challenge-to-cracking-the-coding-interview-fb3f955012e4)

# 算法👨‍🎓

1.  这是在寻找**最大和时使用的类似方法。**只需对**稍加改动。**
2.  当你在寻找所有元素的**和**时，有一个**相似性，**当你加上一个**正数**和就会增加，当你加上一个负数**和就会减少。**
3.  但是对于一组数字的**乘积，当你处理两个负数时，它变成正数，这是你必须考虑的解决这个问题的情况。**
4.  这里我们不仅要存储最大元素**，还要存储最小元素。**
5.  因为 min 元素会变成最大的正数，如果再乘以另一个负数。( **-9 * -1 > -8 * -1)**
6.  max 元素可以是当前元素，也可以是(curr element *前一个 max，curr_element *前一个 min)的 **max。**

7.在遍历结束时，返回 **max** 🔚

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个极其有用的链表面试技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
[亚马逊 SDE 面试前 25 题](https://medium.com/javarevisited/top-25-amazon-sde-interview-questions-cfe0ef70ba9e?source=your_stories_page---------------------------)
[你觉得你真的了解斐波那契数列吗？](https://medium.com/javarevisited/are-you-making-these-fibonacci-number-mistakes-5e3cbedd367e?source=your_stories_page---------------------------)
[用 C 编程解决 9 个最佳字符串问题](https://medium.com/@akshay_ravindran/9-best-strings-problem-solved-using-c-5e2a1d373fc2?source=your_stories_page---------------------------)
[一个不简单的解决 50 个黑客等级挑战](https://medium.com/javarevisited/top-50-coding-challenges-in-hacker-rank-3d79c181528?source=your_stories_page---------------------------)
[15+二叉树问题问在 FAANG](https://medium.com/dev-genius/15-binary-tree-coding-problems-from-faang-interview-2ba1ec67d077?source=your_stories_page---------------------------)

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