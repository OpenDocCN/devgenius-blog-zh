# Leetcode 竞赛 296

> 原文：<https://blog.devgenius.io/leetcode-contest-296-c48a8c476b7d?source=collection_archive---------8----------------------->

![](img/aba3d10f65ecda73fb60cb0fb0b293c0.png)

穆罕默德·拉赫马尼在 [Unsplash](https://unsplash.com/s/photos/coding-competition?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 问题 1

## [最小最大游戏](https://leetcode.com/contest/weekly-contest-296/problems/min-max-game/)

> 给你一个 **0 索引的**整数数组`nums`，其长度是`2`的幂。
> 
> 在`nums`上应用以下算法:
> 
> 设`n`为`nums`的长度。如果`n == 1`，**结束**过程。否则，**创建一个新的长度为`n / 2`的 **0 索引的**整数数组`newNums`。**
> 
> 对于每个**偶数**索引`i`其中`0 <= i < n / 2`，**将`newNums[i]`的值分配给**作为`min(nums[2 * i], nums[2 * i + 1])`。
> 
> 对于每一个**奇数**索引`i`，其中`0 <= i < n / 2`，**将`newNums[i]`的值指定为`max(nums[2 * i], nums[2 * i + 1])`。**
> 
> **用`newNums`替换**数组`nums`。
> 
> **从步骤 1 开始重复**整个过程。
> 
> 应用算法后，返回 `nums` *中剩余的最后一个数字*。**

## **接近**

解决方案是问题中描述的简单实现。

*   对于**偶数**索引，我们取 **nums[2*i]** 和 **nums[2*i + 1]** 中的最小值
*   对于奇数索引**和奇数索引**，我们取最大值 **nums[2*i]** 和 **nums[2*i + 1]**

我们这样做，直到数组中只存在一个数字。

## 解决办法

[](https://github.com/bumblebee211196/lc_contests/blob/main/contest_296/python/problem1.py) [## LC _ contracts/problem 1 . py 在主大黄蜂 211196/LC _ contracts

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/bumblebee211196/lc_contests/blob/main/contest_296/python/problem1.py) 

> 时间复杂度:O(n)
> 
> 空间复杂度:O(n)

# 问题 2

## [划分数组，使最大差值为 K](https://leetcode.com/contest/weekly-contest-296/problems/partition-array-such-that-maximum-difference-is-k/)

> 给你一个整数数组`nums`和一个整数`k`。你可以将`nums`分割成一个或多个**子序列**，这样`nums`中的每个元素都会出现在**的**子序列中。
> 
> 返回*所需的* ***最小*** *个数，使得每个子序列中的最大值和最小值之差最多为***`k`*。***
> 
> ****子序列**是可以通过删除一些或不删除元素而不改变剩余元素的顺序从另一个序列中导出的序列。**

## **方法**

**对元素进行分组的条件是最大和最小元素之间的差必须是 *≤ k* 。我们对数组进行排序，并使用双指针方法，我们固定第一个指针的位置并移动第二个指针，直到两个指针处的元素之差为 *≤ k* 。当条件失败时，我们更新第一个指针的位置并重复这个过程。子序列的数量是第一个指针被更新的次数。**

## **解决办法**

**[](https://github.com/bumblebee211196/lc_contests/blob/main/contest_296/python/problem2.py) [## LC _ contracts/problem 2 . py 在主大黄蜂 211196/LC _ contracts

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/bumblebee211196/lc_contests/blob/main/contest_296/python/problem2.py) 

> 时间复杂度:O(nlogn)
> 
> 空间复杂度:O(1)

# 问题 3

## [替换数组中的元素](https://leetcode.com/contest/weekly-contest-296/problems/replace-elements-in-an-array/)

> 给你一个由`n` **不同的**正整数组成的 **0 索引的**数组`nums`。对该数组应用`m`操作，在`ith`操作中，用`operations[i][1]`替换数字`operations[i][0]`。
> 
> 保证在`ith`操作中:
> 
> `operations[i][0]` **存在于`nums`中的**。
> 
> `operations[i][1]`在`nums`中**不存在**吗？
> 
> 返回*应用所有运算*后得到的数组。

## 方法

我们为给定数组中的每个元素维护一个值到索引的映射器。对于每个更新操作，我们找到**操作[i][0]** 的索引，并更新 **nums[operation[i][0]]** 到**操作[i][1]** 中的值，并更新索引映射器。

## 解决办法

[](https://github.com/bumblebee211196/lc_contests/blob/main/contest_296/python/problem3.py) [## LC _ contracts/problem 3 . py 在主大黄蜂 211196/LC _ contracts

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/bumblebee211196/lc_contests/blob/main/contest_296/python/problem3.py) 

> 时间复杂度:O(n)
> 
> 空间复杂度:O(n)

# 问题 4

## [设计一个文本编辑器](https://leetcode.com/contest/weekly-contest-296/problems/design-a-text-editor/)

> 设计一个带有光标的文本编辑器，它可以执行以下操作:
> 
> **将**文本添加到光标所在的位置。
> 
> **删除光标所在位置的**文本(模拟退格键)。
> 
> **向左或向右移动**光标。
> 
> 删除文本时，只会删除光标左侧的字符。光标也将保留在实际文本中，不能移动到文本之外。更正式的说法是，`0 <= cursor.position <= currentText.length`永远成立。
> 
> 实现`TextEditor`类:
> 
> `TextEditor()`用空文本初始化对象。
> 
> `void addText(string text)`将`text`追加到光标所在的位置。光标在`text`右侧结束。
> 
> `int deleteText(int k)`删除光标左侧的`k`字符。返回实际删除的字符数。
> 
> `string cursorLeft(int k)`向左移动光标`k`次。返回光标左侧的最后一个`min(10, len)`字符，其中`len`是光标左侧的字符数。
> 
> `string cursorRight(int k)`向右移动光标`k`次。返回光标左侧的最后一个`min(10, len)`字符，其中`len`是光标左侧的字符数。

## 方法

我们可以使用两层方法。 *lstack* 和 *rstack* 。光标将始终位于*堆栈*的顶部。

1.  **追加**给定的文本将被追加到*堆栈*中。
2.  **删除**我们从 *lstack* 中删除字符，直到 k 达到 0 并且 *lstack* 不为空。
3.  **向左移动光标**向左移动光标的意思是，移动其下方的顶部指针，只是从 *lstack* 中弹出元素。弹出的元素将被存储在 *rstack* 中。
4.  **向右移动光标**向右移动光标的意思是，移动上方的指针，只是从 *rstack* 中弹出元素。弹出的元素将存储在 *lstack* 中。

## 解决办法

[](https://github.com/bumblebee211196/lc_contests/blob/main/contest_296/python/problem4.py) [## LC _ contracts/problem 4 . py 在主大黄蜂 211196/LC _ contracts

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/bumblebee211196/lc_contests/blob/main/contest_296/python/problem4.py) 

> 时间复杂度:O(n)其中 n 是添加、删除的字符数或光标移动的位置数。
> 
> 空间复杂度:O(n)其中 n 是存储在任一堆栈中的字符总数。

我希望你们喜欢这篇文章。

如果你觉得有帮助，请分享和鼓掌非常感谢！😄

欢迎在评论区提问！。**