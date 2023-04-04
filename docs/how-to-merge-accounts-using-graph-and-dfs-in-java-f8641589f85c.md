# JAVA 中如何使用 Graph 和 DFS 合并账户？

> 原文：<https://blog.devgenius.io/how-to-merge-accounts-using-graph-and-dfs-in-java-f8641589f85c?source=collection_archive---------0----------------------->

## 第 44 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/49ed698924a41f870e3139b6378ff99f.png)

照片由[基奥娜·李](https://unsplash.com/@kionalee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/t/architecture?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

*   出免费故事？下面是我的 [**好友链接。**](https://medium.com/@akshay_ravindran/f8641589f85c?source=friends_link&sk=b4380999461aacf1d9faddf03ae4dc6b)
*   100 天到 LinkedIn，雅虎，甲骨文

# 介绍

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 44 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但是我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，是因为我已经完成了一项挑战[，重点是亚马逊和脸书的面试](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079)。

# 新的一天，新的力量，新的想法

# 第 44 天—账户合并🏁

# 目的

给定一个列表`accounts`，每个元素`accounts[i]`都是一个字符串列表，其中第一个元素`accounts[i][0]`是一个*名字*，其余元素是 *emails* 代表账户的 emails。

现在，我们想合并这些账户。如果两个账户有共同的电子邮件，那么这两个账户肯定属于同一个人。请注意，即使两个帐户有相同的名称，它们也可能属于不同的人，因为人们可能有相同的名称。一个人最初可以有任意数量的账户，但是他们所有的账户都必须有相同的名字。

合并账户后，按照以下格式返回账户:每个账户的第一个元素是姓名，其余元素是排序后的邮件**。帐户本身可以按任何顺序返回。**

# **Example🕶**

```
**Input:** 
accounts = [["John", "johnsmith@mail.com", "john00@mail.com"], ["John", "johnnybravo@mail.com"], ["John", "johnsmith@mail.com", "john_newyork@mail.com"], ["Mary", "mary@mail.com"]]
**Output:** [["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  ["John", "johnnybravo@mail.com"], ["Mary", "mary@mail.com"]]
**Explanation:** 
The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
```

> **关注[**代码之家**](https://medium.com/@akshay_ravindran) **s** 了解编程面试世界的最新动态。**

# **密码👇**

**作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)**

# **算法**

1.  **创建一个 Hashmap，将电子邮件存储到给定的名称中。所以当你有电子邮件来查找名字时，你可以调用这个映射函数。**
2.  **创建一个 HashSet 来存储没有任何重复的可能的电子邮件列表。**
3.  **创建一个图表来存储电子邮件之间的链接。**
4.  **现在你必须填写图表，以便电子邮件之间有一个逻辑联系。**
5.  **遍历每个电子邮件列表。对于列表中的每封邮件，你必须确保它们与同一列表中的所有其他邮件相连接。**
6.  **现在，以 dfs 方式遍历图表，并从起点开始遍历每个电子邮件帐户。那么所有这些链接都被认为是同一个帐户。**
7.  **将路径添加到结果列表中。**
8.  **在遍历结束时，返回列表🔚**

# **复杂性分析**

> ****时间复杂度:O(N Log N)因为排序函数**
> **空间复杂度:O(N)因为额外的 hashmap 和 sets****

**[![](img/ef1fa9bd510e414dce52745663cf74a2.png)](https://amzn.to/3eZbTS9)

感谢你制作了这个排名第一的新版本🖤** 

# **进一步阅读**

**[4 个极其有用的面试链表技巧](https://medium.com/javarevisited/4-incredibly-useful-linked-list-tips-for-interview-79d80a29f8fc?source=your_stories_page---------------------------)
[亚马逊 SDE 面试前 25 题](https://medium.com/javarevisited/top-25-amazon-sde-interview-questions-cfe0ef70ba9e?source=your_stories_page---------------------------)
[你觉得你真的了解斐波那契数列吗？](https://medium.com/javarevisited/are-you-making-these-fibonacci-number-mistakes-5e3cbedd367e?source=your_stories_page---------------------------)
[用 C 编程解决 9 个最佳字符串问题](https://medium.com/@akshay_ravindran/9-best-strings-problem-solved-using-c-5e2a1d373fc2?source=your_stories_page---------------------------)
[一个人不简单地解决 50 个黑客等级挑战](https://medium.com/javarevisited/top-50-coding-challenges-in-hacker-rank-3d79c181528?source=your_stories_page---------------------------)**

# **线的尽头**

**你现在已经到了这篇文章的结尾。谢谢你阅读它。祝你编程面试好运！**

**如果你在面试中遇到这些问题。请在下面的评论区分享它。我会很高兴读到它们。**

**[](https://medium.com/javarevisited/the-ultimate-guide-to-binary-trees-47112269e6fc) [## 二叉树的最终指南

### 任何你必须知道的关于二叉树的事情！

medium.com](https://medium.com/javarevisited/the-ultimate-guide-to-binary-trees-47112269e6fc) 

当我们发布新的编码挑战时，不要忘记点击**关注 button✅** 以接收更新。告诉我们你**是如何解决**这个问题的。🔥我们会很高兴阅读它们。❤:我们可以在一篇博文中介绍你的方法。

> 想在 java 编程方面出类拔萃？

[![](img/b2cb79e739ccbe5267e12198cf85fbe3.png)](https://www.amazon.in/Solved-Programming-Challenges-Coding-Interviews-ebook/dp/B07S5K4Z32/ref=sr_1_1?keywords=100%20best%20solved%20programming%20challenges&qid=1563392111&s=gateway&sr=8-1&source=post_page---------------------------)

已经解决的 **100 个 Java(面试)编程问题**汇编。**(黑客等级)🐱‍💻。**这是完全**免费的**🆓如果你订阅了亚马逊 kindle。

![](img/ceab82d2eddf08386084956ab2306f3b.png)

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)**