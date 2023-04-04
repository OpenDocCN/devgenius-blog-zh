# 日常编码问题:问题 8

> 原文：<https://blog.devgenius.io/daily-coding-problem-problem-8-ec735fee5471?source=collection_archive---------13----------------------->

![](img/09ba9c9c68bbc6f97bc67550345dd656.png)

照片由 [XPS](https://unsplash.com/@xps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/@xps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我又遇到了另一个问题！。

# 问题

> 单值树(代表“通用值”)是一种其下所有节点都具有相同值的树。
> 
> 给定二叉树的根，计算单叶子树的个数。
> 
> 例如，下面的树有 5 个单价子树:

```
 0
  / \
 1   0
    / \
   1   0
  / \
 1   1
```

让我们假设我们的`Node`类型具有以下属性

1.  `val`当前节点的值
2.  `left`指针指向左侧节点
3.  `right`指针指向右边的节点

我们如何检查给定的树是否是单叶树？。如果左节点或右节点的值都与其父节点的值匹配，则称其为单叶树。我们用这个逻辑来检验一棵树是否是单叶树。我们首先检查根节点是否是单叶树。如果是这样，我们增加总数并递归地检查它的左右子树。这将得出给定树中单叶树的总数。

# 解决方案 1:

**Python:**

**开始:**

时间复杂度:O(n)

**空间复杂度:** O(n)

时间复杂度为 O(n)的原因是对于树的每个节点，我们也要再次评估其子树中的每个节点。我们如何将它简化为 O(n)？🤔…为了降低时间复杂性，我们需要以某种方式合并这两种功能的功能。我们可以使用相同的函数来检查它是否是单叶树，并计算单叶树的数量。

# 解决方案 2:

**Python:**

**去:**

时间复杂度: O(n)

**空间复杂度:** O(n)

我希望你们喜欢这篇文章。

如果你觉得有帮助，请分享和鼓掌非常感谢！😄

欢迎在评论区提出你的疑问！。