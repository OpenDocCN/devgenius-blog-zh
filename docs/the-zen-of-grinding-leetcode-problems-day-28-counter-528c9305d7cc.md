# 研磨 LeetCode 问题的禅:第 28 天-计数器

> 原文：<https://blog.devgenius.io/the-zen-of-grinding-leetcode-problems-day-28-counter-528c9305d7cc?source=collection_archive---------9----------------------->

欢迎回到 [**LeetCode 日常练习系列**](https://medium.com/@matei.danut.dm/the-zen-of-grinding-leetcode-problems-day-0-motivation-681842565166) **。**今天我们将讨论使用计数器解决的 **2 个简单的**问题。让我们开始吧！

![](img/d92d867d67daa731850fbdfa8afe8960.png)

# 利用计数器

[](https://leetcode.com/problems/find-the-difference/) [## 找出差异- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/find-the-difference/) 

**见解**:

*   Python Counter 类非常有用，不仅因为它允许你用一行代码来计算幽灵，还因为它覆盖了减法运算符
*   在 Python 中，任何类都可以访问许多名为 [**dunders**](https://realpython.com/operator-function-overloading/) (因为它们看起来像 **__dunder__()** )的特殊方法，这些方法相当于一系列运算符:+、-、/、%、[]、()等。
*   如果您覆盖了其中一个，那么当在该类的一个元素上使用相应的操作符时，它*将被调用。例如， **__sub__()** 会在我们做 **a — b** 时被调用*
*   由于 Counter 如此特殊，减去这个类的两个项目基本上会给我们**额外的元素，这些元素在第一个项目中，而在第二个项目中没有**

# 四分之一多数

[](https://leetcode.com/problems/element-appearing-more-than-25-in-sorted-array/) [## 元素在排序数组中出现超过 25 %- leet code

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/element-appearing-more-than-25-in-sorted-array/) 

反向解决方案

**见解**:

*   Python 计数器再次保存了我们，我们基本上在 O(n)时间内获得了所有元素的计数，所以剩下的就是检查哪个元素满足条件
*   我们还可以检查最常见的元素，因为问题保证了它是出现超过 25%时间的同一个元素

结束语:

*   你有没有过**很长一段时间**休假，而你**完全彻底浪费掉了**？对我来说，这曾经是暑假，我会随意浏览网页，看视频，玩电子游戏。
*   自从我长大后，我从来没有真正有过这么长的连续时间可以做我想做的事情，但每当我有类似假期的事情时，我发现我往往会陷入同样的习惯
*   每当我这样做的时候，我都恨我自己，因为我总是告诉自己，我会在头几天放松一下，然后慢慢变得更有效率。然后，我没有做任何有成效的事情，直到最后我都感到内疚。然后，工作又开始了，我告诉自己*没关系，我需要休息*。
*   但这不是生活的方式，因为我不得不处理让自己失望的罪恶感，这让我精疲力尽。
*   我认为**摆脱这种情况的唯一方法是安排一个真正的假期**，比如去另一个地方做一些事情。或者更经常和我的朋友一起聚会。或者玩一个我非常期待的电子游戏。
*   所以解决办法是按照数百本自我提升书籍告诉我的去做，提前计划好第二天。如果我那样做了，我知道即使我没有 100%按照计划去做，至少我知道我决定做什么**。**