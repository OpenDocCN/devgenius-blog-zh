# Leetcode 竞赛 297

> 原文：<https://blog.devgenius.io/leetcode-contest-297-75150be135ed?source=collection_archive---------9----------------------->

![](img/aba3d10f65ecda73fb60cb0fb0b293c0.png)

照片由[穆罕默德·拉赫马尼](https://unsplash.com/@afgprogrammer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/coding-competition?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 问题 1

## [计算已缴税款](https://leetcode.com/contest/weekly-contest-297/problems/calculate-amount-paid-in-taxes/)

> 给你一个 **0 索引的** 2D 整数数组`brackets`，其中`brackets[i] = [upperi, percenti]`表示`ith`税级的上限为`upperi`，税率为`percenti`。括号按上限排序**(即`upperi-1 < upperi`代表`0 < i < brackets.length`)。**
> 
> 税款计算如下:
> 
> 赚取的第一笔`upper0`美元按`percent0`的税率征税。
> 
> 接下来赚的`upper1 - upper0`美元按`percent1`的税率征税。
> 
> 接下来赚的`upper2 - upper1`美元按`percent2`的税率征税。
> 
> 诸如此类。
> 
> 给你一个整数`income`,代表你赚了多少钱。申报你必须缴税的金额。实际答案的`10-5`范围内的答案将被接受。

## 方法

我们循环通过**括号**，直到括号中提供的收入上限超过我们的收入。我们跟踪以前的收入界限来计算税收等级，当收入上限超过收入时，我们使用最大等级来计算税收和回报。

## 解决办法

[](https://github.com/bumblebee211196/lc_contests/blob/main/contest_297/python/problem1.py) [## LC _ contracts/problem 1 . py 在主大黄蜂 211196/LC _ contracts

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/bumblebee211196/lc_contests/blob/main/contest_297/python/problem1.py) 

> 时间复杂度:O(n)
> 
> 空间复杂度:O(1)

# 问题 2

## [网格中的最小路径成本](https://leetcode.com/contest/weekly-contest-297/problems/minimum-path-cost-in-a-grid/)

> 给你一个由从`0`到`m * n - 1`的**个不同的**个整数组成的 **0 索引的** `m x n`个整数矩阵`grid`。你可以在这个矩阵中从一个单元格移动到下一个行的**中的任何其他单元格。也就是说，如果您在单元格`(x, y)`中，那么`x < m - 1`，您可以移动到单元格`(x + 1, 0)`、`(x + 1, 1)`、...，`(x + 1, n - 1)`。**注意**不能从最后一行的单元格移动。**
> 
> 每个可能的移动都有一个由大小为`(m * n) x n`的 **0 索引的** 2D 数组`moveCost`给出的代价，其中`moveCost[i][j]`是从具有值`i`的单元格移动到下一行的列`j`中的单元格的代价。从最后一行`grid`的单元格开始移动的代价可以忽略。
> 
> `grid`中一条路径的成本是所有被访问的单元格值的**总和**加上所有移动的**总和**。返回从第 ***行第一个*** *行的任意单元格开始，到最后一个* *行的任意单元格结束的路径的****最小*** *成本。***

## *方法*

*假设只有一行，那么结果会很简单，选择网格中的最小值。*

*如果输入有两行，那么对于第二行中的每个值，我们将找到从第一行中的任何单元格到达的最小成本。最终结果将是第二行中每个单元的成本中的最小值。*

*对于网格中的任何单元，我们计算到达它的最小成本。我们对网格中除第一行之外的每个单元格重复这一过程。为了避免重新计算，我们维护了一个 2D 网格来缓存我们的结果。2D 网格最后一行中的最小值将是我们的结果。*

## *解决办法*

*[](https://github.com/bumblebee211196/lc_contests/blob/main/contest_297/python/problem2.py) [## LC _ contracts/problem 2 . py 在主大黄蜂 211196/LC _ contracts

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/bumblebee211196/lc_contests/blob/main/contest_297/python/problem2.py) 

> 时间复杂度:O(m * n)
> 
> 空间复杂度:O(m * n)

# 问题 3

## [公平分配饼干](https://leetcode.com/contest/weekly-contest-297/problems/fair-distribution-of-cookies/)

> 给你一个整数数组`cookies`，其中`cookies[i]`表示`ith`包中的饼干数量。你还会得到一个整数`k`,表示将所有的饼干袋分配给**多少个孩子。同一个袋子里的所有饼干必须给同一个孩子，不能分开。**
> 
> 分配的**不公平性**定义为分配中单个孩子获得的**最大** **总**饼干。
> 
> 返回****最小*** *所有分配的不公平*。*

## *方法*

*这个问题可以用回溯来解决。我们尝试了所有可能的组合来最小化分配的不公平性。*

***注意:**对于 python 来说，天真的回溯将 TLE。因此，需要应用分支的界限。在这种情况下，当不公平性超过先前计算的结果时，我们不进行分支。因为分布中的不公平性已经超过了先前计算的结果，所以转到分支的末端没有意义。*

## *解决办法*

*[](https://github.com/bumblebee211196/lc_contests/blob/main/contest_297/python/problem3.py) [## LC _ contracts/problem 3 . py 在主大黄蜂 211196/LC _ contracts

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/bumblebee211196/lc_contests/blob/main/contest_297/python/problem3.py) 

> 时间复杂度:O(k^n)
> 
> 空间复杂度:O(k)

# 问题 4

## [命名公司](https://leetcode.com/contest/weekly-contest-297/problems/naming-a-company/)

> 您得到了一个字符串数组`ideas`，它表示在命名公司的过程中使用的名称列表。命名公司的过程如下:
> 
> 从`ideas`中选择 2 个**不同的**名称，分别称为`ideaA`和`ideaB`。
> 
> 将`ideaA`和`ideaB`的第一个字母互换。
> 
> 如果新名称的**和**都没有在原`ideas`中找到，那么名称`ideaA ideaB`(`ideaA`和`ideaB`的**串联**，中间用空格隔开)就是有效的公司名称。
> 
> 否则，它不是有效的名称。
> 
> 返回****不同*** *的公司有效名称*。*

## *方法*

*我们把所有的名字按其开头字母分组。以不同字符开头的名字只有在它们的正文不匹配时才能被考虑。*

*比如“coffee”和“donuts”有不同的起始字符和不同的体“c-offee”和“d-onuts”。交换这里的起始字符就产生了两个不同的单词“c-onuts”和“d-offee”。然而，“咖啡”和“太妃糖”有不同的起始字符，但具有相同的体“c-offee”和“t-offee”。交换起始字符不会产生不同的单词。*

*我们可以使用一个**设置**和**差值**操作来消除这种情况。*

## *解决办法*

*[](https://github.com/bumblebee211196/lc_contests/blob/main/contest_297/python/problem4.py) [## LC _ contracts/problem 4 . py 在主大黄蜂 211196/LC _ contracts

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/bumblebee211196/lc_contests/blob/main/contest_297/python/problem4.py) 

> 时间复杂度:O(m)其中 m 是相同起始字符下的主体的最大数量。这个时间复杂度主要是由于集差运算。
> 
> 空间复杂度:O(n)

我希望你们喜欢这篇文章。

如果你觉得有帮助，请分享和鼓掌非常感谢！😄

欢迎在评论区提问！。***