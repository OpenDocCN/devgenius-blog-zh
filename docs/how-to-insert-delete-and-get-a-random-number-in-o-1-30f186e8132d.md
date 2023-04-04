# 如何在 O(1)中插入、删除、获取一个随机数？

> 原文：<https://blog.devgenius.io/how-to-insert-delete-and-get-a-random-number-in-o-1-30f186e8132d?source=collection_archive---------13----------------------->

## 第 12 天——100 天到 LinkedIn、雅虎、甲骨文

![](img/1d7b277b2c6d7418a3f89a2818971ec2.png)

来自 [Pexels](https://www.pexels.com/photo/background-clips-eraser-notebook-584305/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Tirachard Kumtanom](https://www.pexels.com/@tirachard-kumtanom-112571?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 摄影

*   出免费故事？下面是我的 [**好友链接。**](https://medium.com/dev-genius/how-to-insert-delete-and-get-a-random-number-in-o-1-30f186e8132d?source=friends_link&sk=fe2fc25f3e7617c25f241b6f9f2a782f)
*   100 天到 LinkedIn，雅虎，甲骨文

# Introduction🛹

嘿，伙计们，今天是 LinkedIn 挑战 100 天的第 12 天。

[![](img/5717f22d4f252dc4852215f164d1c376.png)](https://amzn.to/3fLILh7)

Kindle 阅读器免费

如果你在准备面试。即使你已经在工作中安顿下来，让自己了解最新的面试问题对你的职业发展是至关重要的。从这里的**开始你的**准备**！**

上个月，我一直在研究这些公司的常见问题。我已经收集了这些问题中的 100 个，我不能保证你会在面试中得到这些问题，但是我相信这些“面试问题”中的大多数都有相似的逻辑，并且从这些挑战中运用了相同的思维方式。

在我们进入第一个问题之前，如果你想知道我为什么选择 LinkedIn、雅虎和甲骨文而不是 FAANG，那是因为我已经完成了一项挑战，重点是这个系列中的亚马逊和脸书面试问题:

[](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) [## 亚马逊 100 天—第一天

### 亚马逊最常问的 100 个问题。代码和方法的解释。

medium.com](https://medium.com/javarevisited/100-days-to-amazon-day-1-b9e07228f079) 

# 新的一天，新的力量，新的想法🚀

# 第 12 天—插入、删除、获取随机 O(1)🏁

# 目的🏹

设计一个在*平均* **O(1)** 时间内支持所有后续操作的数据结构。

1.  `insert(val)`:将项目值插入集合(如果不存在的话)。
2.  `remove(val)`:从集合中删除一个项目值(如果存在)。
3.  `getRandom`:从当前元素集中随机返回一个元素(保证调用该方法时至少有一个元素存在)。每个元素必须有相同的返回概率。

# Example🕶

```
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```

# 密码👇

作者:[阿克谢·拉文德兰](https://www.linkedin.com/in/akshay-ravindran-096)

# 算法👨‍🎓

1.  说到 O(1) insert 本身，首先想到的是一个 **HashMap。**
2.  在这里，我实现了一个**列表**来跟踪当前元素，还实现了一个**散列表**来保存**元素**及其在列表中各自的位置( **index)** 。

**在 HashMap 中插入是 O(1)，在列表末尾也是 O(1)。**

1.  当**插入**函数被调用时，检查**中的**元素是否已经存在，如果是则**返回 false。**
2.  否则将元素添加到**映射**中，key 是元素，value 是列表的**大小。**

**删除(棘手的部分):**

1.  我们只能在**O(1)**中删除列表的结尾，这不会妨碍进一步的删除。
2.  打个鬼主意，如果我们在**中间**删除列表会怎么样，是不是**索引的**在**删除后**会自动改变**渲染**hashmap 中的**对**没用。
3.  因此，我们必须找到一种方法，总是让**确保**删除发生在列表的**末端。**
4.  我所做的是，如果它不是列表中的**最终元素**。**将**最后一个元素复制到**当前**索引并删除列表的结尾。
5.  这将确保列表中的索引值与 hashmap 中的值相似。

**获得随机**

1.  有一个名为 **nextInt(int n)** 的 Math.random 函数将在 **O(1)中给出一个特定范围内的随机数。**
2.  你所要做的就是将 nextInt 的值作为**列表**的**索引**，你将从这里**返回**元素。

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