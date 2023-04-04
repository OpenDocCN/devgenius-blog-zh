# 2021 年三月糖果挑战——问题 1:分发糖果

> 原文：<https://blog.devgenius.io/march-leetcoding-challenge-2021-problem-1-distribute-candies-f37f66ea7ee9?source=collection_archive---------2----------------------->

我们将解决 2021 年 3 月远程编码挑战的第一个问题

![](img/5a3400005d076e695f51912c0594b465.png)

詹姆斯·哈里森在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# **问题陈述**

爱丽丝有`n`糖果，其中`ith`糖果的类型是`candyType[i]`。爱丽丝注意到她开始发胖，所以她去看医生。

医生建议爱丽丝只吃她所有的糖果中的`n / 2`(`n`总是偶数)。爱丽丝非常喜欢她的糖果，她想在遵循医生建议的同时吃尽可能多的不同类型的糖果。

给定长度为`n`的整数数组`candyType`，返回*如果她只吃*中的 `n / 2` *种糖果，她可以吃的最大* *种糖果的数量。*

**例 1:**

```
**Input:** candyType = [1,1,2,2,3,3]
**Output:** 3
**Explanation:** Alice can only eat 6 / 2 = 3 candies. Since there are only 3 types, she can eat one of each type.
```

**例 2:**

```
**Input:** candyType = [1,1,2,3]
**Output:** 2
**Explanation:** Alice can only eat 4 / 2 = 2 candies. Whether she eats types [1,2], [1,3], or [2,3], she still can only eat 2 different types.
```

**例 3:**

```
**Input:** candyType = [6,6,6,6]
**Output:** 1
**Explanation:** Alice can only eat 4 / 2 = 2 candies. Even though she can eat 2 candies, she only has 1 type.
```

# **解决方案**

在这里，我们必须找到不同类型的糖果的最大数量，可以按照约束条件来吃。解决方案非常简单。

首先，我们将计算不同糖果的总数。如果这个数小于`total amount of candies/2`，我们可以直接退回。如果它大于 return `total amount of candies/2`因为它们会不同。

时间复杂度:O(n)，n 是数组大小

空间复杂度:O(n)，n 是数组大小

代码可以在这里找到

[](https://github.com/sksaikia/LeetCode/tree/main/src/MarchLeetcodeChallenge2021) [## sksaikia/LeetCode

### 在 GitHub 上创建一个帐户，为 sksaikia/LeetCode 开发做贡献。

github.com](https://github.com/sksaikia/LeetCode/tree/main/src/MarchLeetcodeChallenge2021)