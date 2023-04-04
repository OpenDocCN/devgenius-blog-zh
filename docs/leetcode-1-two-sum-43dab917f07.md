# LeetCode : 1—两个和(用图像获得解)

> 原文：<https://blog.devgenius.io/leetcode-1-two-sum-43dab917f07?source=collection_archive---------1----------------------->

链接:→[https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)

![](img/1a39a1a384f015be20e9f5ba1e9d4dce.png)

照片由 [Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# **问题:**

给定一个整数数组`nums`和一个整数`target`，返回这两个数的索引*，使它们相加为* `*target*`。

你可以假设每个输入都有 ***恰好*一个解**，你不能两次使用*相同的*元素。

可以任意顺序返回答案。

# **回答:**

现在，我们必须在数组中的所有数字中找到两个数字，这样当它们相加时，它们会给我们第三个数字，等于`target`。

例如，

```
**Input:** nums = [2,7,11,15], target = 9
**Output:** [0,1]
**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].
```

有两种方法，让我们首先讨论简单的一种:

## **方法 1**

*   取一个元素
*   将此元素与所有其他元素相加
*   相加后，将总和与给定的目标值进行比较
*   如果总和等于目标值，则返回这两个元素的索引
*   如果总和不等于目标，我们检查下一对

这看起来很简单，但让我们分析一下时间和空间的复杂性。

*   ***时间复杂度***

假设数组中有`n`元素-

```
For 1st element - we will check for (n - 1) elements
For 2nd element - we will check for (n - 2) elements
For 3rd element - we will check for (n - 3) elements 
and so on...
```

所以最终结果我们可以得到如下

```
[(n - 1) + (n - 2) + (n - 3) + ... + 2 + 1]
```

现在，如果我们把这些物品重新排序，第一个之后是最后一个，然后是第二个，然后是倒数第二个

```
[(n - 1) + 1 + (n - 2) + 2 + (n - 3) + 3 ...]
```

物品的排列方式现在你可以看到每一对都等于

```
[(n - 1 + 1) = n, (n - 2 + 2) = n, ...
```

由于有 n-1 个项目，所以有(n-1)/2 个这样的对。

所以你要加 n(n-1)/2 次，所以总值是

`N*(N-1)/2`。

```
n * (n — 1) / 2 = n2–2n ≈ n2
```

因此，我们的时间复杂度将是— ***O(n2)*** 。

*   ***空间复杂度***

由于我们没有使用任何额外的数据结构，因此我们的空间复杂度将是 *O(1)* 。

## 方法 2

当我们分析这个问题时，我们遇到了下面的等式

```
c = a + b
```

但是我们能把上面的等式写成-

```
b = c - a
```

*   遍历数组
*   检查我们以前是否遇到过当前元素
*   如果有，我们将返回这个元素的索引以及`target`和`current element`的差的索引。只有当我们保存了`target`和当添加到当前元素时给出`target`本身的元素的差异时，我们才会遇到这个元素。
*   如果没有，我们将保存`target`和`current element`的差值。
*   重复这个过程

因为我们需要存储我们的`difference`，所以我们需要一个地图数据结构来存储`difference`及其相应的索引。

*   ***时间复杂度***

因为我们只对数组迭代一次，所以时间复杂度是 ***O(n)* 。**

*   ***空间复杂度***

由于我们需要一个数组大小的映射，空间复杂度将是 ***O(n)* 。**

**代码(Python):**

我希望你喜欢这篇文章。在这里，我们解决了在 *O(n)* 时间和 *O(n)* 空间中关于 LeetCode 的第一个问题。

感谢你阅读这篇文章，❤

如果我做错了什么？让我在评论中。我很想进步。

拍手声👏如果这篇文章对你有帮助。