# 如何用两个栈求解 Max Stack？

> 原文：<https://blog.devgenius.io/how-to-solve-max-stack-using-two-stacks-c4ac4cc77922?source=collection_archive---------4----------------------->

## 第 42 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/2772a4248a617c85d5487438a5613c9a.png)

照片由[王思然·哈德森](https://unsplash.com/@hudsoncrafted?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/stack?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

*   出于免费的故事？这里是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-solve-max-stack-using-two-stacks-c4ac4cc77922?source=friends_link&sk=f795ab1a4d6174e189f7c7923bdd97ac)
*   100 天到 LinkedIn，雅虎，甲骨文

# 介绍

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 42 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从**这里**开始你的**准备**！

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战[重点是亚马逊和脸书的面试](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)。

# 新的一天，新的力量，新的想法

# 第 42 天—最大堆栈🏁

# 目的

设计一个支持 push，pop，top，peekMax，popMax 的 max 栈。

1.  push(x) —将元素 x 推送到堆栈上。
2.  pop() —移除堆栈顶部的元素并将其返回。
3.  top() —获取顶部的元素。
4.  peekMax() —检索堆栈中的最大元素。
5.  popMax() —检索堆栈中的最大元素，并将其移除。如果您找到多个最大元素，只删除最上面的一个。

# Example🕶

```
MaxStack stack = new MaxStack();
stack.push(5); 
stack.push(1);
stack.push(5);
stack.top(); -> 5
stack.popMax(); -> 5
stack.top(); -> 1
stack.peekMax(); -> 5
stack.pop(); -> 1
stack.top(); -> 5
```

> 关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法

1.  创建两个堆栈。一个表示数字序列的流入。而另一个堆栈表示该元素的最大元素。
2.  对于推送操作()。检查传入元素是否大于当前堆栈的最大元素。如果是这样，更新 maxStack，然后将它压入主栈。否则就把它推到主栈。
3.  对于 Pop()，只需从两个堆栈中弹出顶部的元素。
4.  对于 Peek()。从主堆栈中返回顶部元素。
5.  对于 PopMax()。你必须从 max 栈中获取 max 元素。现在，在主堆栈中，弹出元素，直到到达 max 元素。
6.  请记住，我们必须只弹出 max 元素，所以所有这些弹出的值应该以相同的顺序放回堆栈中。
7.  为了做到这一点，当你从主堆栈中弹出元素时，把它添加到一个临时缓冲区。
8.  从主堆栈中移除 max 元素后，将临时缓冲区中的所有元素再次推入主堆栈。
9.  返回最大元素🔚

# 复杂性分析

> **时间复杂度:O(1)除了 popMax()是 O(N)**
> **空间复杂度:O(N)对于栈的大小**

[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤

# 进一步阅读

[4 个非常有用的面试链表技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
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

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 以接收更新。告诉我们你**是如何解决**这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费的**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)