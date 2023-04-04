# 研磨 LeetCode 问题的禅:第 26 天-另一种媒介

> 原文：<https://blog.devgenius.io/the-zen-of-grinding-leetcode-problems-day-26-another-medium-da5008fb1eda?source=collection_archive---------10----------------------->

欢迎回到 [**LeetCode 日常练习系列**](https://medium.com/@matei.danut.dm/the-zen-of-grinding-leetcode-problems-day-0-motivation-681842565166) **。**我们将讨论 **2 易**题和 **1 中题。让我们开始吧！**

![](img/090028dcaa631bfb6dd874e78c2f6e85.png)

照片由 [Geordanna Cordero](https://unsplash.com/@geordannatheartist?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 第一个唯一字符

[](https://leetcode.com/problems/first-unique-character-in-a-string/) [## 字符串中的第一个唯一字符- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/first-unique-character-in-a-string/) 

**见解:**

*   另一个提醒是在你需要的时候使用 Python 的**计数器**集合
*   还要使用 Python 的 **enumerate** 结构，因为这是获取集合中项目的索引和值的最 Python 化的方式

# 算出偶数总数

[](https://leetcode.com/problems/count-integers-with-even-digit-sum/) [## 用偶数和 LeetCode 计数整数

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/count-integers-with-even-digit-sum/) 

**见解**:

*   这个问题最困难的部分是计算出当数字是偶数时和奇数时的区别
*   过了这一步，就只需要根据数字本身找到正确的公式了

# 从字母到数字

[](https://leetcode.com/problems/reconstruct-original-digits-from-english/) [## 从英文字母代码重建原始数字

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/reconstruct-original-digits-from-english/) 

**见解:**

*   考虑这个问题有点难度，但是实现起来很容易
*   主要思路是弄清楚这样一个事实:有些字母在所有数字中只出现一次，例如在**零**中，**只出现**一次，在**两个**中， **w** 只出现*一次。这适用于全部 10 个中的 5 个*
*   在删除与我们刚刚识别的数字相对应的字母后，现在我们注意到这样一个事实，即另一组字母在剩余的可能数字中只出现一次
*   最后，不属于这些集合的**的最后一个数字*是‘T50’九个***。所以剩下的字母“I”的个数等于 9 的个数。
*   因此，我们知道字符串中每个数字有多少

结束语:

*   我倾向于享受那些我必须像解决难题一样去解决的问题:从直觉开始，然后一步一步地建立起来
*   这与**中的**相比，问题很容易解释，但是**中的**实现起来很繁琐:大量的边缘案例、一个接一个的错误、大量的变量和涉及的指数
*   这可能源于这样一个事实，有时我容易被**压倒**。因此，理论上我知道如何解决问题，这一事实让我期望实践部分也同样简单
*   在现实世界中，有时你不得不接受这样的事实:实现**可能需要一些时间**，并且充满了无限的循环和错误。这是程序员的一部分，即使人们不太喜欢调试，如果你一次只专注于一个问题，你会度过难关的。
*   所以我认为在这方面做得更好的一个好方法是**更多地关注中等问题**。即使我没有完成它们，我至少应该尝试在给定的时间框架内做尽可能多的事情