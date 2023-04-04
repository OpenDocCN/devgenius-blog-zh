# CS50 决选

> 原文：<https://blog.devgenius.io/cs50-runoff-elections-36a0f37808c3?source=collection_archive---------1----------------------->

![](img/f808e2429373ce9f31679a4b0591de1c.png)

直接进入 [**功能**](#0a0f) 。

嗨，这是我对 CS50 问题集 3 中给出的径流程序的解决方案，因为在所有的编码中，可能还有很多其他更好的方法来解决这个问题。

让我们从最上面开始看代码，我们必须包含 string.h 库，然后为我们定义 MAX _ 选民和 MAX _ 候选者，它们是宏定义`#define`允许它们的定义，它们被用作常数，不能被我们的代码改变

这里声明了二维数组、每个候选对象的结构、数组候选对象、投票者计数和候选对象计数。可视化 2D 阵列对我来说有点困难，它们基本上是行和列，我是这样理解的:

在这种情况下，我们有 3 名候选人和 3 名选民。[MAX _ 选民]这是行数，[MAX _ 候选者]是列数

```
VOTERS  | CANDIDATES
voter[0]|[0][1][2]
voter[1]|[2][0][1]
voter[2]|[1][2][0]
```

在这之后，我们有了我们的功能原型和主要开始。

这里主要是开始，当我们编译我们的程序时，我们需要输入候选人，我们输入的候选人数量将存储在`argc`中，候选人的名字将存储在数组`argv[]`中。如果我们没有输入任何候选人或者候选人数量大于最大值，前两次`if`检查中的一次将失败。如果这些都通过了，我们就开始填充候选人的数组。

循环中的`for`将迭代每个候选项，添加到`struct`并将其附加到候选项的数组中，名称的分配类似于`argv[i + 1]`，因为当我们运行程序时`argv`也将我们的`./runoff`添加到数组中，所以我们需要跳过它。

如果投票人数超过 100 人，我们就返回。

然后，我们有我们的嵌套,`for loops`第一个迭代通过投票者的数量，第二个迭代通过候选人的数量，并且它用`vote`存储投票，我们下面要做的一个功能是:[i]是投票者，[j]是用户选择作为`n`等级的候选人。

最后的循环

这个循环一直运行到，`print_winner` 或`is_tie` 返回 true，为了让`is_tie`打破这个循环，剩下的所有候选人需要有相等的票数，`Tabulate`计算每个候选人的票数。在所有的函数运行之后，每个候选人的票数被重置为零，并且调用`tabulate`函数重新开始计数。

# **功能**

我们的第一个函数是`vote`函数。该函数有三个参数，`i`作为投票者，`j`作为等级，而`name` 是适当等级的候选人。

`for`循环遍历候选人数组中的每个候选人，它使用函数`srtcmp`将用户输入的姓名与候选人数组中的`candidates[i]`进行比较。如果它们相等，它就用用户选择候选人的投票人和排名索引更新 preferences 数组。

在这里，我打印了每个程序要求候选人的投票人和每个候选人的排名，只是为了看看它如何以更好的方式工作。

如果候选人在上一轮循环中没有被淘汰，则`tabulate`函数会增加每个候选人的票数。它通过在嵌套的`for`循环中遍历每个投票者和候选人来完成，然后我们得到在`c`等级上的候选人，检查它是否被淘汰，如果没有，给该候选人添加投票，我们跳出循环，继续同一个投票者

重要的是要理解，如果这个候选人被淘汰，例如，如果只有一个投票人将其作为等级 1，则等级 2 现在变成等级 1，以此类推，则只有等级 1 的候选人的票数增加。

现在我们有了`print_winner`功能，我认为这是最简单的一个，在我看来，其他的都很难。

我们需要记住，要在决选中宣布获胜者，获胜者必须赢得至少一半的选票，这是我们在这里实现的。

我们遍历每个候选人，检查他们的票数是否大于候选人总数除以二(一半)，如果是这样，函数打印获胜者并返回`true`，否则返回`false`。

对于`find_min`函数，我们创建了一个包含两个数字的数组。你也可以用一个数字变量，我只是觉得这样更容易理解，我们迭代每个候选人，如果`smallest[0]`大于当前候选人的票数，并且还没有被淘汰，就把`smallest[0]`的值改成那个数。该函数遍历所有候选人，然后返回票数最少的候选人。

`is_tie`函数对我来说是最难理解的，下面是我对它的解构；当 while 循环运行并且还没有赢家时，我们用`is_tie`检查。在函数中，我们创建两个变量，一个存储有多少候选人拥有最低票数，另一个存储有多少候选人仍在竞选，然后我们编写一个`for`循环遍历每个候选人。如果候选人尚未被淘汰，并且拥有最低票数，我们递增这两个变量，`else if`候选人尚未被淘汰，但没有最低票数，这意味着候选人仍在竞选，因此我们递增`onRun`变量。

最后，如果`tie_count`与`onRun`的数量相同，则函数返回 true，否则返回 false。

我们需要排除得票最少的候选人，并考虑选民选择的第二个候选人，这模拟了如果得票最少的候选人一开始就没有参加选举会发生什么。

该函数遍历每个候选项，如果候选项的票数最少，它将被删除，切换我们的 struct 候选项所具有的 bool `eliminated`。

感谢您的阅读，如果您有任何疑问，请告诉我，我会尽力帮助您！！

```
:) runoff.c exists
:) runoff compiles
:) vote returns true when given name of candidate
:) vote returns false when given name of invalid candidate
:) vote correctly sets first preference for first voter
:) vote correctly sets third preference for second voter
:) vote correctly sets all preferences for voter
:) tabulate counts votes when all candidates remain in election
:) tabulate counts votes when one candidate is eliminated
:) tabulate counts votes when multiple candidates are eliminated
:) tabulate handles multiple rounds of preferences
:) print_winner prints name when someone has a majority
:) print_winner returns true when someone has a majority
:) print_winner returns false when nobody has a majority
:) print_winner returns false when leader has exactly 50% of vote
:) find_min returns minimum number of votes for candidate
:) find_min returns minimum when all candidates are tied
:) find_min ignores eliminated candidates
:) is_tie returns true when election is tied
:) is_tie returns false when election is not tied
:) is_tie returns false when only some of the candidates are tied
:) is_tie detects tie after some candidates have been eliminated
:) eliminate eliminates candidate in last place
:) eliminate eliminates multiple candidates in tie for last
:) eliminate eliminates candidates after some already eliminated
```