# 如何解决雨水截留问题？

> 原文：<https://blog.devgenius.io/how-to-solve-trapping-rain-water-problem-1e43dcc78101?source=collection_archive---------6----------------------->

## 第 25 天 LinkedIn、雅虎、甲骨文的 100 天

![](img/31401829ad66cfb49e1e96dea2ac1eeb.png)

来自 [Pexels](https://www.pexels.com/photo/silhouette-and-grayscale-photography-of-man-standing-under-the-rain-1530423/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的[亚历山大帕萨里奇](https://www.pexels.com/@apasaric?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

*   走出免费故事？这里是我的[**Friend Link**T7](https://medium.com/@akshay_ravindran/1e43dcc78101?source=friends_link&sk=5e073736921b8034fc75b0d7c561fc8f)
*   LinkedIn、雅虎、甲骨文上市 100 天

# Introduction🛹

各位，今天是 LinkedIn 天挑战赛的第 25 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你正在准备面试。即使你已经在工作中安顿下来，让自己了解最新的**面试问题**对于你的**职业**成长**也是必不可少的。从**开始你的**准备**在这里**！**

上个月，我一直在研究，以找出这些公司的常见问题。我已经整理了这些问题中的 **100** ，我并不是在向你保证，你会在面试中得到这些问题，但我相信，这些“面试问题”中的大多数都有着相似的逻辑，并从这些挑战中运用了相同的思维方式。

在我们继续讨论第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我完成了一项挑战 [**聚焦亚马逊和脸书访谈**](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) **。**

# 新的一天，新的力量，新的想法🚀

# 第 25 天—截留雨水🏁

# 目的🏹

给定 *n* 个表示高程图的非负整数，其中每个条的宽度为 1，计算雨后其能够截留多少水。

![](img/8c3c8f8bac5846231423d314bbbf3631.png)

# Example🕶

```
**Input:** [0,1,0,2,1,0,1,3,2,1,2,1]
**Output:** 6
```

> 跟随[**书屋**](https://medium.com/@akshay_ravindran) 的 **s** 在编程面试圈中与时俱进。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法👨‍🎓

1.  创建两个数组来表示左右高度。
2.  将左数组初始化为第一个元素，将右数组的最后一个元素初始化为高度数组的最后一个元素。
3.  现在，为了从头开始填充左侧数组遍历，将最大值放在高度数组中的当前元素和左侧数组中的前一个元素之间。
4.  类似地，通过从右向左遍历来填充右边的数组。
5.  最后，遍历两个数组，找出两个高度(左、右)的最小值之间的差异，并从(高度数组)中减去该值。将所有这些差异相加(计数)。
6.  返回 count 的值。🔚

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

[](https://medium.com/javarevisited/100-days-to-linkedin-challenge-10d84a92b63f) [## LinkedIn 挑战赛 100 天

### 第 1 天-嵌套列表重量总和

medium.com](https://medium.com/javarevisited/100-days-to-linkedin-challenge-10d84a92b63f) 

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 接收更新。告诉我们你是如何解决这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费的**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)