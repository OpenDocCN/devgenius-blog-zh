# 2021 年 3 月电子编码挑战—第 15 天:编码和解码 TinyURL

> 原文：<https://blog.devgenius.io/march-leetcoding-challenge-2021-day-15-encode-and-decode-tinyurl-bfeeba44308e?source=collection_archive---------9----------------------->

今天，我们将解决三月电码挑战的第 15 个问题。

![](img/0c6f55dc912c21d861717bcc6cfd7b30.png)

# 问题陈述

TinyURL 是一个 URL 缩短服务，你输入一个 URL，比如`https://leetcode.com/problems/design-tinyurl`，它会返回一个简短的 URL，比如`[http://tinyurl.com/4e9iAk](http://tinyurl.com/4e9iAk.)` [。](http://tinyurl.com/4e9iAk.)

为 TinyURL 服务设计`encode`和`decode`方法。对编码/解码算法的工作方式没有任何限制。你只需要确保一个 URL 可以被编码成一个微小的 URL，并且这个微小的 URL 可以被解码成原始的 URL。

# 解决办法

在这个问题中，我们必须设计一个 URL 缩短服务来编码和解码 URL。因此，对于给定的`longURL` ，我们应该能够用`encode` 方法得到`shortURL` ，反之亦然。

所以我们可以使用内置的`hashcode`方法，因为这将为任何给定的字符串生成一个惟一的值。我们可以将`hashcode`值存储在`HashMap`中，并在 decode 方法中检索它。

代码如下。

代码可以在这里找到

[](https://github.com/sksaikia/LeetCode/tree/main/src/MarchLeetcodeChallenge2021) [## sksaikia/LeetCode

### 在 GitHub 上创建一个帐户，为 sksaikia/LeetCode 开发做贡献。

github.com](https://github.com/sksaikia/LeetCode/tree/main/src/MarchLeetcodeChallenge2021) 

看看我在 2021 年 3 月 LeetCoding 挑战赛上的其他帖子。

1.  [三月吃糖挑战——第一天——分发糖果](https://medium.com/dev-genius/march-leetcoding-challenge-2021-problem-1-distribute-candies-f37f66ea7ee9)
2.  [三月 LeetCoding 挑战赛—第 2 天—设置不匹配](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-2-set-mismatch-4abd5ee491c9)
3.  [三月电码挑战赛—第三天—缺少号码](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-3-missing-number-ae8ee45a58cb)
4.  [三月 LeetCoding 挑战赛—第 4 天—两个链表的交集](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-4-intersection-of-two-linked-lists-a775449b5563)
5.  [三月 LeetCoding 挑战赛—第 5 天—二叉树平均水平](https://link.medium.com/sC9L595opeb)
6.  [三月电子编码挑战—第 6 天—单词的短编码](https://medium.com/leetcode-simplified/march-leetcoding-challenge-2021-day-6-short-encoding-of-words-7fed4bfae557)
7.  [三月电子编码挑战—第 7 天—设计散列表](https://levelup.gitconnected.com/leetcode-706-design-hashmap-march-leetcoding-challenge-2021-fdae1a4adbc)
8.  [三月 LeetCoding 挑战——第 8 天——移除回文子序列](https://sourav-saikia.medium.com/march-leetcoding-challenge-2021-day-8-remove-palindromic-subsequences-12b037705722)
9.  [三月 LeetCoding 挑战赛——第 10 天——整数变罗马](https://medium.com/leetcode-simplified/march-leetcoding-challenge-2021-day-10-integer-to-roman-76caa87a0e07)
10.  [三月 LeetCoding 挑战赛——第 12 天——检查一个字符串是否包含所有大小为 K 的二进制代码](https://medium.com/dev-genius/march-leetcoding-challenge-2021-day-12-check-if-a-string-contains-all-binary-codes-of-size-k-48f1fed4d9b9)
11.  [三月 LeetCoding 挑战赛——第 14 天——交换链表中的节点](https://medium.com/leetcode-simplified/march-leetcoding-challenge-2021-day-14-swapping-nodes-in-a-linked-list-a540785c816f)