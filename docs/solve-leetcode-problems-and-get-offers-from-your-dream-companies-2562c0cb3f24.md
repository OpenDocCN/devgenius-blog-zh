# 解决 Leetcode 问题，获得你梦想中的公司的报价

> 原文：<https://blog.devgenius.io/solve-leetcode-problems-and-get-offers-from-your-dream-companies-2562c0cb3f24?source=collection_archive---------1----------------------->

1423.**您可以从卡中获得的最高点数**

在这个系列中，我将和我的朋友一起解决 Leetcode 媒体问题，你可以在我们的 youtube 频道上看到，今天我们将解决问题 Leetcode: 1423。**您可以从卡片中获得的最高点数。**

![](img/dea39cffd7c4ce1d5b747677e98ef1b8.png)

稍微介绍一下我，我过去有来自**优步**印度和**亚马逊**印度的 offers，目前在阿姆斯特丹**Booking.com**工作。

# 学习算法的动机

[](https://medium.com/leetcode-simplified/solve-leetcode-problems-and-get-offers-from-your-dream-companies-2786415be0b7) [## 解决 Leetcode 问题，获得你梦想中的公司的报价

### 练习 Leetcode 背后的介绍和动机

medium.com](https://medium.com/leetcode-simplified/solve-leetcode-problems-and-get-offers-from-your-dream-companies-2786415be0b7) 

# 问题陈述

有几张卡片**排成一行**，每张卡片都有相关的点数，点数在整数数组`cardPoints`中给出。

在一个步骤中，您可以从该行的开头或结尾抽取一张牌。你得拿正好`k`张牌。

你的分数是你所拿的牌的分数之和。

给定整数数组`cardPoints`和整数`k`，返回你可以获得的最大分数*。*

***例 1:***

```
***Input:** cardPoints = [1,2,3,4,5,6,1], k = 3
**Output:** 12
**Explanation:** After the first step, your score will always be 1\. However, choosing the rightmost card first will maximize your total score. The optimal strategy is to take the three cards on the right, giving a final score of 1 + 6 + 5 = 12.*
```

***例 2:***

```
***Input:** cardPoints = [2,2,2], k = 2
**Output:** 4
**Explanation:** Regardless of which two cards you take, your score will always be 4.*
```

***例 3:***

```
***Input:** cardPoints = [9,7,7,9,7,7,9], k = 7
**Output:** 55
**Explanation:** You have to take all the cards. Your score is the sum of points of all cards.*
```

***例 4:***

```
***Input:** cardPoints = [1,1000,1], k = 1
**Output:** 1
**Explanation:** You cannot take the card in the middle. Your best score is 1.*
```

***例 5:***

```
***Input:** cardPoints = [1,79,80,1,1,1,200,1], k = 3
**Output:** 202*
```

# ***Youtube 讨论***

# ***解决方案***

*我们将讨论两种方法，第一种方法将给出超过的**时限，第二种方法将被接受。***

*根据问题，我们可以从开头或结尾选择一张卡片，我们需要重复这个步骤，直到我们得到 k 张卡片。*

***基础案例:***

1.  *如果`k`小于 1，则不能选择任何牌*

*2.`i`应始终小于`j`*

*3.如果`i`等于`j`和`k=1`，那么我们要选择`i` -th 卡。*

***递归步骤:***

*所以对于从`i`到`j`的数组，有`k`张牌要抽，*

*如果我们从一开始就选择卡片，那么`ar[i]+choose from (i+1) to j`*

*如果我们从末尾选择卡片，那么`ar[j]+choose from (i) to (j-1)`*

*为了避免同样的计算，我们可以使用动态规划。我们可以将`i to j`的值与要在字典中抽取的`k`卡片一起存储。*

*这种解决方案效率不高，并且会超出**的时间限制。***

*先说这个问题的最优解。我们必须从开头或结尾开始打`k`牌。因此我们可以选择*

*`0 cards from left and k cards from right`*

*`1 card from left and (k-1) cards from right`*

*`2 cards from left and (k-2) cards from right`*

*`.............`*

*`k cards from left and 0 cards from right`*

*因此，我们需要找到这些组合的最大值。为了使我们的计算更简单，我们可以存储从开始到结束的前 k 张卡片的累积和。我们可以找到所有这些组合的最大值；*

*`left[i]+right[k-i]`对于`i`的所有可能值(`k`给出)*

*这个问题的代码可以在下面的库中找到。*

*[](https://github.com/webtutsplus/LeetCode/tree/main/src/LC1423_MaximumPointsObtainedFromCards) [## webtutsplus/LeetCode

### 在 GitHub 上创建一个帐户，为 webtutsplus/LeetCode 的开发做出贡献。

github.com](https://github.com/webtutsplus/LeetCode/tree/main/src/LC1423_MaximumPointsObtainedFromCards)* 

# *感谢您的阅读，并关注此出版物以了解更多 LeetCode 问题！😃*

*[](https://medium.com/leetcode-simplified) [## LeetCode 简体

### 我们将现场解决 Leetcode 问题，您可以在我们的 youtube 频道上观看

medium.com](https://medium.com/leetcode-simplified)*