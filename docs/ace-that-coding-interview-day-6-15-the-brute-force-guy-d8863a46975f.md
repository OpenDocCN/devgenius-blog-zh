# 一周的 LeetCode(第 6/7 天)

> 原文：<https://blog.devgenius.io/ace-that-coding-interview-day-6-15-the-brute-force-guy-d8863a46975f?source=collection_archive---------3----------------------->

欢迎第六次参加我们的技术面试准备系列。我们的目标是通过为您提供涵盖许多主题的真正的解决问题的技能，为您轻松获得梦想中的工作铺平道路。

别忘了查看之前的文章:

*   [日(1/15):基础算术](/15-days-of-leetcode-ace-that-coding-interview-day-1-15-9e2a76f5a62e)
*   [日(2/15):二分球技术](/15-days-of-leetcode-ace-that-coding-interview-day-2-15-fc47b4c8996)
*   [日(3/15):贪婪的思维](/15-days-of-leetcode-ace-that-coding-interview-day-3-15-86cd0c1939f5)
*   [*(4/15 日):二分搜索法+ BSGS +馅饼*](/ace-that-coding-interview-day-4-15-binary-search-bsgs-pie-fc5fb0a8a37e)
*   [*日(5/15):* 算算吧！](/ace-that-coding-interview-day-5-15-do-the-math-bea2b1e545d7)
*   ***(6/15 日):蛮干的家伙***
*   [*天(7/15):* 几何](/ace-that-coding-interview-day-7-15-geometry-166564e38d94)

让我们不要浪费时间，直接开始吧！

**一个悲伤的小故事:**

在整个系列中，我们几乎总是将任何暴力算法描述为一个坏家伙，具有可怕的时间复杂性，这通常是真的，所以今天我们反驳这种想法，并展示一些暴力家伙有用的情况。

![](img/cec14a846aeb60c058de7793e81c9b9f.png)

版权 HBO

蛮力并不是一个有着众所周知的步骤的特定算法。其实是一种穷尽所有结果解决问题的方式。

第六天:[回文分割](https://leetcode.com/problems/palindrome-partitioning/)

我假设你没有阅读任务描述，所以这里有一个摘要:

> *给定一个字符串 S，把它分割成若干子串，使它们覆盖整个字符串，每个子串都是回文。*
> 
> 返回所有有效分区的集合。
> 
> 一个**回文**字符串是一个向后读和向前读一样的字符串。

**示例**:

S = "aab "

第一分区:“a”、“a”、“b”
第二分区:“aa”、“b”

一般的经验法则是:如果问题约束很小，你可能应该打电话给暴力破解者，记住给定字符串的长度是 16，这相当小，所以我们在这里打电话给那个人。

现在让我们看看在这种情况下我们的人会怎么做:

因为分区必须覆盖整个字符串，所以分区的第一部分必须是它的前缀(给定的字符串)，所以我们只尝试所有的前缀。

现在我们有了第一个片段，让我们用 k 来表示它的长度，我们忽略 S 的前 k 个字符，从 S 的第(k+1)个字符开始寻找第二个片段，最终，我们将得到一个覆盖所有 S 的有效分区。

让我们尝试一个例子来澄清任何疑问。

S = "aba "

第一个子串“a”、“ab”或“aba”有 3 种选择，但是只有“a”和“aba”可以使用，因为“ab”不是回文。

我们采用第一个选项，将“a”附加到分区，现在 S 变成了“ba ”,回想一下，我们忽略了已经使用过的字符。

然后类似地，我们有两个可能的前缀(或者“b”或者“ba”)，但是只有“b”是一个回文，我们再次将它附加到分区，所以我们剩下 S =“a”，我们不再有选择，只能将它附加到分区，现在我们有了第一个分区[“a”，“b”，“a]]。

现在让我们跳回到我们做的第一个选择，选择“aba”而不是“a”作为第一个片段，这样我们就有了我们的第二个分区[“ABA”]。

**伪代码:**

```
bruteforce(S, cur_partition)
   answer = 0 
   n = length(S) if S is empty 
      append the partition to the list of answers
      return   try all possible prefixes p_1, p_2,... ,p_n
      if p_i is palindrome
         bruteforce( S[i+1,n], cur_partition + p_i ) 

   return answer That is all we need, let's write some code!
```

C++实现

Python 实现

**向蛮干的家伙学习什么？**

*   将你手中的问题分解成更小的子问题:这种技术非常有用，因为更小的问题往往有更简单、有时是众所周知的解决方案。
*   思考如何高效地解决更小的子问题，并将其组合成一个最终的快乐解决方案。
*   考虑到约束很小，穷尽所有可能的输出在实践中并不坏。
*   重读这篇文章很多次，直到你完全欣赏它。

# 轮到你了:

恭喜你！如果你一口气做到了，我知道你能做到。

既然你有希望获得一些新技能，尤其是 ***蛮干*** ，不要就此打住，把它们付诸实践吧。努力做一个蛮力的家伙；)

**练习附加题:**

[子集](https://leetcode.com/problems/subsets/)

[组合和](https://leetcode.com/problems/combination-sum/)

[含金量最高的路径](https://leetcode.com/problems/path-with-maximum-gold/)

[所有子集 XOR 总和的总和](https://leetcode.com/problems/sum-of-all-subset-xor-totals/)

[排列](https://leetcode.com/problems/permutations/)

[![](img/b0375c5e503d841e11d8f6d055b45292.png)](https://www.buymeacoffee.com/Mohamed.Sobhy)

有问题可以在 [**LinkedIn**](https://www.linkedin.com/in/mohamed-sobhy-12181b165/) 上联系我，在 [**中**](https://medium.com/@mohamedsobhi777) 和 [**GitHub**](https://github.com/mohamedsobhi777) 上关注我更多有趣的文章。

今天到此为止。感谢阅读！