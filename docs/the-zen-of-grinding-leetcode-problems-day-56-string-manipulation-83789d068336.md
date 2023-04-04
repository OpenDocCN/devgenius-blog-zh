# 破解 LeetCode 问题的禅:第 56 天——字符串操作

> 原文：<https://blog.devgenius.io/the-zen-of-grinding-leetcode-problems-day-56-string-manipulation-83789d068336?source=collection_archive---------6----------------------->

欢迎回到 [**LeetCode 日常练习系列**](https://medium.com/@matei.danut.dm/the-zen-of-grinding-leetcode-problems-day-0-motivation-681842565166) **。**今天我做了**2**易题。我们开始吧！

![](img/9eda4fb40958d08929d7a0b9e35af6b3.png)

图为 [Kier 在](https://unsplash.com/@kierinsight?utm_source=medium&utm_medium=referral)上看到 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 删除字符串中所有相邻的重复项

[](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/) [## 删除 String - LeetCode 中所有相邻的重复项

### 给你一个由小写英文字母组成的字符串 s。重复删除包括选择两个相邻的…

leetcode.com](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/) 

**见解**:

*   考虑字符串的两个部分比操作一个索引更容易。因此，左边的部分将没有相邻的副本，而右边的部分将被顺序处理。
*   相邻的重复项只能作为左边部分的最后一个元素出现，后跟右边部分的第一个元素。每当这种情况出现时，我们就把两者都去掉，如果新的相同的字符接触到，我们就继续这样做
*   最后，我们返回左边的部分

# 反转字符串中的元音

[](https://leetcode.com/problems/reverse-vowels-of-a-string/) [## 字符串的反向元音- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/reverse-vowels-of-a-string/) 

**见解**:

*   一种方法是从字符串两端的两个索引开始，交换我们找到的每对元音。虽然这可能会更有效，但为了更清晰，我选择了不同的解决方案
*   我们正在做的是识别元音的索引，将它们放在一个列表中，反转它们，最后将原来的元音分配到这些新的位置
*   由于 Python 中的字符串是不可变的，我们必须使用第 11 行的语法来“改变”一个字符。实际上，这构建了一个全新的字符串，可能会很慢，但是在我们的例子中，它符合时间限制

结束语:

*   当**硬拉**时，如果你在开始动作前没有完全支撑好，那就特别*危险。如果你在举起相当大的重量时*不够投入*，一些肌肉(或者更糟，你的脊椎)会让你失望。**做或不做，没有尝试**。*
*   当然，在健身房这很简单，你有其他更安全的选择来训练同样的肌肉，给或拿。但在生活中，有时你必须硬拉，不管你愿不愿意。你必须大胆去应对强加给你的困难时期。你必须集中全力去达到你想要的结果。
*   就我个人而言，我在这个世界上的四分之一世纪里没有处理过高风险的情况。我不得不努力工作来实现我的一些最大的目标，但是一旦你付出了 T4 的努力，这些目标就一定会实现。
*   如果你去健身房，你不可能不增加肌肉。如果你一丝不苟地学习编程，你不可能找不到一份好工作。如果你限制支出，你就不可能破产。
*   但是，如果你自己创业，将所有的钱投资于股票或资产，或者朝着一个更加非传统的梦想努力，你可能会失败。
*   而且**没有后备计划真的让我害怕**。浪费了我多年的生命和大量的金钱，结果却比我开始的地方还要糟糕，从个人角度来说，这将是灾难性的。我的意思是，我可以把一切都重建起来，但我会落后，人们会看到这一点，我会看起来像一个傻瓜，没有什么好理由就把这一切都抛弃了。
*   尽管如此，我觉得你不能只从确定的事情中构建你的生活。有时候，你可能不得不*停止试图控制一切*和*寻找外部担保*并采取勇敢的行动。我需要在这方面做得更好，无论如何。