# 2021 年 3 月 LeetCoding 挑战—第 19 天:钥匙和房间

> 原文：<https://blog.devgenius.io/march-leetcoding-challenge-2021-day-19-keys-and-rooms-d7ad63a89f45?source=collection_archive---------4----------------------->

今天，我们将解决三月电码挑战的第 19 个问题。

![](img/f236ea99911ca24a3818b19e66fba28c.png)

# 问题陈述

有`N`个房间，你从房间`0`开始。在`0, 1, 2, ..., N-1`每个房间都有一个独特的号码，每个房间可能都有一些钥匙可以进入下一个房间。

形式上，每个房间`i`都有一个键`rooms[i]`列表，每个键`rooms[i][j]`都是`[0, 1, ..., N-1]`中的整数，其中`N = rooms.length`。一把钥匙`rooms[i][j] = v`打开号码为`v`的房间。

最初，所有房间开始锁定(除了房间`0`)。

你可以自由地在房间之间来回走动。

返回`true`当且仅当你能进入每个房间。

**例 1:**

```
**Input:** [[1],[2],[3],[]]
**Output:** true
**Explanation:** 
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3\.  Since we were able to go to every room, we return true.
```

**例二:**

```
**Input:** [[1,3],[3,0,1],[2],[0]]
**Output:** false
**Explanation:** We can't enter the room with number 2.
```

**注:**

1.  `1 <= rooms.length <= 1000`
2.  `0 <= rooms[i].length <= 1000`
3.  所有房间的钥匙总数最多为`3000`。

# 解决办法

在这个问题中，我们必须找到是否可以访问所有的房间。最初，`0-th`房间是打开的，在每个索引处，我们都有进入其他房间的钥匙。所以，我们可以把这个问题想象成一个基于图的问题。

一旦我们进入`0-th`房间，我们将检查那个房间的钥匙。一旦我们找到钥匙，我们就直接去那个房间，重复这个过程。我们在一个`HashSet`中保存已访问房间的数量。如果 HashSet 的大小等于房间的数量，我们返回 true，否则返回 false。我们正在使用呼吸优先搜索来解决这个问题。

代码可以在这里找到

[](https://github.com/sksaikia/LeetCode/tree/main/src/MarchLeetcodeChallenge2021) [## sksaikia/LeetCode

### 在 GitHub 上创建一个帐户，为 sksaikia/LeetCode 开发做贡献。

github.com](https://github.com/sksaikia/LeetCode/tree/main/src/MarchLeetcodeChallenge2021) 

看看我在 2021 年 3 月 LeetCoding 挑战赛上的其他帖子。

1.  [三月吃糖挑战——第一天——分发糖果](https://medium.com/dev-genius/march-leetcoding-challenge-2021-problem-1-distribute-candies-f37f66ea7ee9)
2.  [三月电码挑战—第二天—设置不匹配](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-2-set-mismatch-4abd5ee491c9)
3.  [三月电码挑战赛—第三天—缺少号码](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-3-missing-number-ae8ee45a58cb)
4.  [三月电子编码挑战—第 4 天—两个链表的交集](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-4-intersection-of-two-linked-lists-a775449b5563)
5.  [三月 LeetCoding 挑战赛—第 5 天—二叉树平均水平](https://link.medium.com/sC9L595opeb)
6.  [三月电子编码挑战赛——第 6 天——单词的短编码](https://medium.com/leetcode-simplified/march-leetcoding-challenge-2021-day-6-short-encoding-of-words-7fed4bfae557)
7.  [三月电子编码挑战—第 7 天—设计散列表](https://levelup.gitconnected.com/leetcode-706-design-hashmap-march-leetcoding-challenge-2021-fdae1a4adbc)
8.  [三月 LeetCoding 挑战—第 8 天—移除回文子序列](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-8-remove-palindromic-subsequences-12b037705722)
9.  [三月 LeetCoding 挑战赛—第 10 天—整数变罗马](https://medium.com/leetcode-simplified/march-leetcoding-challenge-2021-day-10-integer-to-roman-76caa87a0e07)
10.  [三月 LeetCoding 挑战赛—第 12 天—检查字符串是否包含所有 K 大小的二进制代码](https://medium.com/dev-genius/march-leetcoding-challenge-2021-day-12-check-if-a-string-contains-all-binary-codes-of-size-k-48f1fed4d9b9)
11.  [三月 LeetCoding 挑战赛——第 14 天——交换链表中的节点](https://medium.com/leetcode-simplified/march-leetcoding-challenge-2021-day-14-swapping-nodes-in-a-linked-list-a540785c816f)
12.  [3 月 LeetCoding 挑战赛—第 15 天—编码和解码 TinyURL](https://medium.com/dev-genius/march-leetcoding-challenge-2021-day-15-encode-and-decode-tinyurl-bfeeba44308e)
13.  [三月 LeetCoding 挑战—第 18 天—摆动序列](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-18-wiggle-subsequence-3c408287325b)