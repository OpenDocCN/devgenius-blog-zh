# 破解 LeetCode 问题的禅:第 63 天——使用 SQL 日期

> 原文：<https://blog.devgenius.io/the-zen-of-grinding-leetcode-problems-day-63-working-with-sql-dates-f37e2367d890?source=collection_archive---------15----------------------->

欢迎回到 [**LeetCode 日常练习系列**](https://medium.com/@matei.danut.dm/the-zen-of-grinding-leetcode-problems-day-0-motivation-681842565166) **。**今天我做了**2**易题。我们开始吧！

![](img/d9f9c622782cc9ec131e8d03d022da27.png)

由[窗口](https://unsplash.com/@windows?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 过去 30 天的用户活动

[](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/) [## 过去 30 天的用户活动 I - LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/) 

**见解**:

*   请注意，您可以将日期表示为字符串。虽然建议使用 **to_date** 函数将其转换为日期类型，但通常这种表示也可以
*   另外，如果在 LeetCode 上处理日期，记得使用 **substr** 字符串函数。否则，您还将拥有默认时间信息，即 00:00:00

# 销售分析 III

[](https://leetcode.com/problems/sales-analysis-iii/) [## 销售分析三- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/sales-analysis-iii/) 

**见解**:

*   注意， *d1* 和*D2*之间的**日期语法等同于使用 ***日期* ≥ *d1* 和日期≤ *d2*** 语法。你喜欢哪个就用哪个。**
*   另外，请注意，我们需要一个**中间表**来获取产品的最小和最大销售日期。这是因为我们需要*和*都在给定的范围内。

结束语:

*   每当你和你的朋友在一起时，你会比独自一人时更喜欢社交，也更快乐。那么*哪个*才是**真正的你**？
*   我曾经说过，很明显，当你独自一人时，你的行为方式就是你真正的个性。我的意思是，你的*朋友不能和你一起*去面试、约会或做一个困难的陈述。最后你**还得能靠自己**。
*   但是如果你花太多时间独处，你会变得更加焦虑，更加害怕，总体来说更不安全。尽管如此，如果你有大问题，很可能你的一些朋友会主动帮助你。所以你会不习惯现实，你并不完全孤独*。*
*   *其他人认为真正的你是不受约束的版本。因此，如果你能够*将你周围的每个人都视为你的朋友*，与*将每个人都视为竞争对手相比，你会更接近事实。**
*   *根据你的成长方式，你可能会**强调一个现实而不是另一个**。“我们当然是在竞争，我的意思是，如果你不比其他人更好，你就进不了大学，找不到工作，你就无法建立一个好的企业”。“我们当然应该合作，我的意思是这就是社交、教学和工作”。*
*   *我对倾向于第一个现实感到内疚，所以我必须努力承认真相在中间的某个地方。老实说，我认为第二种想法更健康。然而，我不能肯定这是绝对的真理，因为到目前为止，竞争给我带来了很多好东西。*