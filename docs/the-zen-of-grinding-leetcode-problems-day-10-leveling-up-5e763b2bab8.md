# 研磨 LeetCode 问题的禅:第 10 天——升级

> 原文：<https://blog.devgenius.io/the-zen-of-grinding-leetcode-problems-day-10-leveling-up-5e763b2bab8?source=collection_archive---------12----------------------->

还在这里，还在做 [**LeetCode 日常练习系列**](https://medium.com/@matei.danut.dm/the-zen-of-grinding-leetcode-problems-day-0-motivation-681842565166) **。**今天我想试试我的运气，我取得了好成绩，完成了 **1 道中等**题和 **4 道简单**题。

![](img/d412f1ddcbfc4ea8553d576bdfa0f07d.png)

我真的很享受暗黑破坏神 3 的时光，所以升级后的声音永远留在了我的脑海里

# 媒介就是信息

[](https://leetcode.com/problems/break-a-palindrome/) [## 破解回文- LeetCode

### 给定一个由小写英文字母组成的回文字符串，用任何小写字母替换一个字符…

leetcode.com](https://leetcode.com/problems/break-a-palindrome/) 

**见解**:

*   那么，是什么让一个中等问题成为中等问题呢？在这种情况下，是**更高数量的边缘情况**
*   不是说需要你非常擅长算法什么的，只是你**真的要把事情想透**；否则，面试官会觉得你很粗心，写的代码有问题
*   对于这个问题，我不得不**检查**当我只有一个字母时会发生什么，我不得不**检查**当只有 a 时会发生什么，我不得不**检查**如果字符串的长度是奇数会发生什么等等…
*   如果你以前参加过编码面试，你可以证明很多事情都是你**询问关于问题约束**的澄清性问题
*   出于这个原因，当我做中型问题时，我采取更保守的方法:在提交问题之前，我自己通过编辑器*运行一些**小测试用例***

# 一个很好的生成器理解

[](https://leetcode.com/problems/find-the-highest-altitude/) [## 找到最高的高度- LeetCode

### 有一个骑自行车的人要去公路旅行。公路旅行由不同海拔的 n + 1 个点组成。摩托车手开始…

leetcode.com](https://leetcode.com/problems/find-the-highest-altitude/) 

**见解**:

*   这可以用很多方法来完成，例如通过 for 循环，但我喜欢**生成器理解**，在这种情况下，它只是工作
*   我把它分成了 2 行**和**，这样你只需看一眼*就能知道代码做了什么，而不用担心第 3 行*

# 好问题，M.A.A.D .描述

[https://leet code . com/problems/minimum-cost-to-move-chips-to-same-position/](https://leetcode.com/problems/minimum-cost-to-move-chips-to-the-same-position/)

**见解**:

*   在你阅读解决方案之前，试着自己仔细阅读问题陈述，并找出你将如何解决它
*   许多问题被归入简单的范畴，因为一旦你理解了 T2 背后的技巧，实现就只有几行了
*   请记住，在面试过程中，你不会知道问题是*容易*、*中等*还是*难*，所以不要被你看到的吓倒

# 我想念 NumPy

[](https://leetcode.com/problems/maximum-population-year/) [## 最大人口年- LeetCode

### 你得到了一个 2D 整数数组日志，其中每一个都表示这个人的出生和死亡年份。…的人口

leetcode.com](https://leetcode.com/problems/maximum-population-year/) 

**见解**:

*   *如果允许我使用 **NumPy，第 8 行***会更好读；它将允许我通过使用+操作符向范围内的所有年份加一
*   此外，*第 8 行*会激怒任何以前使用过 NumPy 的人，因为如果你不得不经常增加范围，你**永远不会用列表**来做
*   但是，唉，我决定离开它香草
*   我还决定包含一个不错的小空间优化，当我可以只使用 **100** 时，不创建 **2000** 元素数组。

# 最后一个不错

[](https://leetcode.com/problems/rearrange-spaces-between-words/) [## 重新排列单词之间的空格- LeetCode

### 给你一串单词，这些单词被放在一定数量的空格中。每个单词由一个或多个…

leetcode.com](https://leetcode.com/problems/rearrange-spaces-between-words/) 

**见解**:

*   这个问题不是特别有趣，但是我对我写的代码非常满意
*   我觉得很少会有**变量名**是 *100%自描述的*；在现实世界中，你宁愿得到一个简短的变量名，然后得到一个解释它的文档字符串，这违背了 [**干净的编码实践**](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) ，但是你学会了忍受它
*   我喜欢你从*第 12* 行开始的方式，它让你去了解*第 9 行和第 10 行*中的变量，这些变量提示你检查*第 3 行和第 4 行*
*   我想我对解决一个简单问题的简单代码太过于狂热了，但是嘿，你必须时常为这些东西感到骄傲

结束语:

*   从现在开始，我要争取每天 *1* ***中等*** *+ 1* ***容易*** *题*。应该没那么难，但是我相信总有一天我会失败
*   当不超过晚上 12 点的时候，解决问题和写文章会容易得多，所以我想我会继续这样做
*   如果你检查一下 LeetCode 上的问题数量，你会注意到大约有 **500 个简单的**、 **1200 个中等的**和 **500 个困难的**。所以你可以想象在面试中你会遇到什么问题
*   有趣的是，我真的不打算很快加入 **FAANG** 。我喜欢我的工作，我喜欢远程工作，我不想搬迁。也许我应该开始一个 ***Kaggle 系列*** 下一个或者一些与 ***云认证*** 相关的东西，因为这些是我更关心的事情