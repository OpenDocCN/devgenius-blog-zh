# 破解 LeetCode 问题的禅:第 4 天——伟大的日子

> 原文：<https://blog.devgenius.io/the-zen-of-grinding-leetcode-problems-day-4-great-days-2442239aa4?source=collection_archive---------13----------------------->

新的一天，新的一美元。到现在，你已经知道这个练习了:[**【leet code 日常练习系列】**](https://medium.com/@matei.danut.dm/the-zen-of-grinding-leetcode-problems-day-0-motivation-681842565166) **。我今天有很多要写的，因为我在 30 分钟内总共做了 5 道简单的题，比我以前做的多了一倍多。** 我不想把这篇文章写得太长，所以我会试着给每个**一个**至理名言**。**

![](img/2fba6fec0596e68a0cf5cbf7abc3a1bf.png)

# 重复子字符串

[](https://leetcode.com/problems/repeated-substring-pattern/submissions/) [## 重复子串模式- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/repeated-substring-pattern/submissions/) 

目标是看一个字符串是否可以通过多次重复它的一个子字符串来获得。我所做的是首先计算字符串长度的**约数**，并依次将每个约数视为子字符串的长度。

我的解决方案:

***至理名言*** :

*   我本可以更懒，尝试所有可能的子串长度，但是我决定付出一点努力
*   在面试中，**提及两种解决方案**，告诉他**你认为哪个更有效**，问他*他是否希望你实施更难的一个*。
*   有时他会对简单的解决方案感到满意，但还是会留下深刻的印象。

# 检查单词和

[](https://leetcode.com/problems/check-if-word-equals-summation-of-two-words/) [## 检查单词是否等于两个单词的总和- LeetCode

### 字母的字母值是它在字母表中从 0 开始的位置(即' a '--> 0，' b '--> 1，' c '--> 2 等)。)…

leetcode.com](https://leetcode.com/problems/check-if-word-equals-summation-of-two-words/) 

你必须取两个单词，根据规则将它们转换成一个数字，然后检查它们的和是否等于第三个字符串的转换。

我的解决方案:

***至理名言*** :

*   **了解**语言*确实有助于编写简洁的代码*
*   为了使解决方案如此简短，我需要事先了解的事情有:[**list comprehensions**](https://realpython.com/list-comprehension-python/)、 [**ord()函数**](https://docs.python.org/3/library/functions.html) ，从**字符串列表**转换为整数比从**整数列表**转换为整数更容易

# 删节句子

[](https://leetcode.com/problems/truncate-sentence/) [## 截断句子- LeetCode

### 句子是由单个空格分隔的单词列表，没有前导空格或尾随空格。每个…

leetcode.com](https://leetcode.com/problems/truncate-sentence/) 

我们来做个实验，我不会**不给你问题描述**，我只会**给你看解答**，你要**猜**。这是:

有点难做，不是吗？

这个怎么样:

***至理名言*** :

*   不要总是写简短的俏皮话，因为你想让人们认为你很聪明。这真的伤害了**可读性**和**可维护性**
*   如果你有一个好的理由，你可以打破这个规则，比如如果你需要 ***重要的*** 优化，如果一个 liner 足够短**保证没有 bug**，如果你团队的**编码实践**鼓励它

# 数组中的峰值索引

[](https://leetcode.com/problems/peak-index-in-a-mountain-array/) [## 山脉中的峰值指数- LeetCode

### 让我们称数组 arr 为山，如果以下性质成立:给定一个整数数组 arr，它保证…

leetcode.com](https://leetcode.com/problems/peak-index-in-a-mountain-array/) 

想一想 [**二分搜索法**](https://en.wikipedia.org/wiki/Binary_search_algorithm) ，但是你在搜索索引 I 这样一个[i-1] < a[i] > a[i+1]

我的解决方案:

**见解:**

*   二分搜索法不是一个很难理解或实现的算法，但是你真的必须对极限情况了如指掌
*   花些时间自己试验一下，**看看哪些情况你通常会忘记包括**和**为什么**它们很重要
*   **思维练习**:如果你把第 8 排换成**左= m+1** ，把第 10 排换成**右= m-1** ，我的解决方案还有效吗？尝试用纸和笔从短到长的数组开始检查

# 第 k 个缺少正数

[](https://leetcode.com/problems/kth-missing-positive-number/) [## 第 k 个缺少正数- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/kth-missing-positive-number/) 

你有一个数组丢失了一些数字。找到第 k 个丢失的**。**

我的解决方案:

**见解**:

*   再次，**获得一些用 Python 编程的实践经验**
*   在做挑战的时候，我时间很紧，所以在 range()函数 I **内硬编码了值 2001** (根据问题约束)
*   但是好的程序员回去 [**修理他们弄坏的窗户**](https://blog.codinghorror.com/the-broken-window-theory/)

结束语:

*   感觉今天我*太幸运*了，可能*不配*解决这么多问题
*   但是我知道成熟的做法是**承认好的结果并继续前进**
*   如果你想成为那种**不为输**找借口的人，那么**就不要为赢**找借口
*   任何领域的改进都是这样的:**一直不好** - > **一直不好** - > **一直好** - > **一直好**
*   真正帮助我的是**了解这门语言以及它能做什么**；在 C++中，这些问题中的一些会花费更长的时间，但是当面试官给你一个问题时，他会意识到这一点
*   我最初想把今天的文章命名为“*幸运之星*”，但这是对成就的淡化，所以请欣赏 Jojo 的引用