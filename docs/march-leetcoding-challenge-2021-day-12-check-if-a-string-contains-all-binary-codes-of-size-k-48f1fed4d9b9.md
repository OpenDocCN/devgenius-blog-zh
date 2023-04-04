# 2021 年 3 月 LeetCoding 挑战赛——第 12 天:检查一个字符串是否包含所有 K 大小的二进制代码

> 原文：<https://blog.devgenius.io/march-leetcoding-challenge-2021-day-12-check-if-a-string-contains-all-binary-codes-of-size-k-48f1fed4d9b9?source=collection_archive---------6----------------------->

今天，我们将解决三月电码挑战的第 12 个问题。

![](img/73b6244e409667ddae9525f0df99401e.png)

# 问题陈述

给定一个二进制字符串`s`和一个整数`k`。

如果长度为`k`的每个二进制代码都是`s`的子串，则返回 *True* 。否则，返回*假*。

**例 1:**

```
**Input:** s = "00110110", k = 2
**Output:** true
**Explanation:** The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indicies 0, 1, 3 and 2 respectively.
```

**例 2:**

```
**Input:** s = "00110", k = 2
**Output:** true
```

**例 3:**

```
**Input:** s = "0110", k = 1
**Output:** true
**Explanation:** The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring.
```

**例 4:**

```
**Input:** s = "0110", k = 2
**Output:** false
**Explanation:** The binary code "00" is of length 2 and doesn't exist in the array.
```

**例 5:**

```
**Input:** s = "0000000001011100", k = 4
**Output:** false
```

# 解决办法

在这个问题中，我们被要求找出在一个给定的字符串中是否有所有大小为 k 的二进制码。

我们知道，对于一个给定的大小 k，我们可以有一个总数为`2^k`的数。我们需要检查所有这些`2^k`数字是否都出现在给定的字符串中。为此，我们可以使用一个`HashSet`，来存储我们已经找到的所有值。最后，我们可以比较`HashSet`和`2^k`的大小，如果它们相等，则返回 true，否则返回 false。

代码如下。

时间复杂度:O(n)

空间复杂度:O(n)

代码可以在这里找到

[](https://github.com/sksaikia/LeetCode/tree/main/src/MarchLeetcodeChallenge2021) [## sksaikia/LeetCode

### 在 GitHub 上创建一个帐户，为 sksaikia/LeetCode 开发做贡献。

github.com](https://github.com/sksaikia/LeetCode/tree/main/src/MarchLeetcodeChallenge2021) 

看看我在 2021 年 3 月 LeetCoding 挑战赛上的其他帖子。

1.  [三月吃糖挑战——第一天——分发糖果](https://medium.com/dev-genius/march-leetcoding-challenge-2021-problem-1-distribute-candies-f37f66ea7ee9)
2.  [三月 LeetCoding 挑战赛—第 2 天—设置不匹配](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-2-set-mismatch-4abd5ee491c9)
3.  [三月电码挑战赛—第三天—缺少号码](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-3-missing-number-ae8ee45a58cb)
4.  [3 月 LeetCoding 挑战赛—第 4 天—两个链表的交集](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-4-intersection-of-two-linked-lists-a775449b5563)
5.  [三月 LeetCoding 挑战赛—第 5 天—二叉树平均水平](https://link.medium.com/sC9L595opeb)
6.  [三月电子编码挑战—第 6 天—单词的短编码](https://medium.com/leetcode-simplified/march-leetcoding-challenge-2021-day-6-short-encoding-of-words-7fed4bfae557)
7.  [三月 LeetCoding 挑战—第 8 天—移除回文子序列](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-8-remove-palindromic-subsequences-12b037705722)
8.  [三月 LeetCoding 挑战赛——第 10 天——整数变罗马](https://medium.com/leetcode-simplified/march-leetcoding-challenge-2021-day-10-integer-to-roman-76caa87a0e07)