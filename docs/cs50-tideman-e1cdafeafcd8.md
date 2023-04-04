# CS50 Tideman

> 原文：<https://blog.devgenius.io/cs50-tideman-e1cdafeafcd8?source=collection_archive---------0----------------------->

![](img/a66ea6780abaac85f99fce3cfa85ce65.png)

伙计，这个问题很难，我在解决它之前写了这个，我想我可以通过写我正在做的事情来更好地理解这个问题，我建议你做一些类似的事情，在某种意义上是给我某种责任，因为现在我必须完成这篇文章。

让我们从第一个名为`vote`的函数开始，这个函数有三个参数(rank，name 和 ranks)，所有的数据都是从 main 中第二个嵌套的 for 循环中提取的，`rank`是当前被迭代的候选项的索引，`name`用户选择的候选项的名称，`ranks`用户如何对候选项进行排序，这是我们在这个函数中要解决的问题。

我们遍历候选人数组，如果数组中的一个名字与用户输入的名字相同，我们用名字输入的索引更新`ranks`数组。例如，如果我们有候选人`Alice Bob Charlie`，他们的索引是`[0 1 2]`，如果用户投票给`Rank 1: Alice | Rank 2: Charlie | Rank 3: Bob`，那么`Ranks`数组在第一个投票者之后看起来就像这个`[0 2 1]`。

现在，`record_preferences`函数将填充`preferences`数组`preferences[i][j]`中候选人的数量，这些候选人更喜欢候选人`i`而不是`j`

这个问题的逻辑很让人沮丧，也很难。根据演练视频，首选项数组如下所示:

```
candidates
Alice Bob Charlie

voter preferences, n candidate is represented by n row
Alice  [0 3 2]
Bob    [2 0 3]
Charlie[1 4 0]
there is one preference array for each candidate
```

我们需要创建一个嵌套的 for 循环，这是我们开始循环的第二个循环，`i+1`，因为我们只需要在`j`跟在`i`后面时添加到 preferences 数组中。我建议您浏览`debug50`来更好地理解这个问题，您可以清楚地看到数字是如何添加到首选项数组中的

我开始讨厌这个问题以前的都很好玩，但是这个 idk 男，不过还是继续用`add_pairs`功能吧。这个函数添加到`pairs`数组中，该数组由具有一个`winner`和一个`loser` `int`值的`pair`结构组成，我们需要更新这个数组，我们还需要更新`pair_count`变量。

我们需要与前面的函数相同的嵌套 for 循环，因为我们不想单独比较候选对象，也不想重复检查。

所以我们写了一个条件来比较哪个候选人比下一个候选人更受欢迎。如果我们有三个候选人`alice bob char`和三个选民，`[i] [j]` `[0][1]`将是第一个，`[j][i]` `[1][0]`。将要比较的值是`3 > 0`，这里所有三个投票者都更喜欢`candidate[i or 0]`而不是`candidate[j or 1]`，所以我们将`[i]`加到`pairs[0].winner`，将`[j]`加到`pairs[0].loser`，并将下面的`pair_count`加 1，这是我们用来访问`pairs`的索引的变量，如果条件为假，则我们检查它是否小于，并且获胜者是`[j]`。这根据我们下面的选民偏好:

```
Rank 1: alice
Rank 2: bob
Rank 3: charRank 1: alice
Rank 2: char
Rank 3: bobRank 1: char
Rank 2: alice
Rank 3: bobThe preferences array would look like this: 
[0 3 2]                                                                      
[0 0 1]
[1 2 0]
```

现在，我们被要求对配对进行排序，我们使用`sort_pairs`函数，我们需要根据胜利的强度对候选人进行排序，我们再次使用嵌套的 for 循环，但是第一个循环是向后的，是降序的，第二个是“正常的”，在 if 语句中，我们转到`preferences`数组的位置`[pairs.winner][pairs.loser]`，它给出了更喜欢某个候选人的人数。

我们将使用**冒泡排序**对数组进行排序，注意`i`像我们说的那样向下，因为在数组的每一次遍历中，我们需要少查看一个索引。

比较`pairs`数组中每对相邻结构的胜利强度，如果配对和位置`[j+1]`的胜利强度大于`[j]`的胜利强度，则使用临时变量`temp`交换配对

下一个函数`lock_pairs`的目的是填充`locked`二维数组。

在此之前，我创建了一个名为`cycle`的新函数，如果创建了一个循环，它将返回 **true** ，我们运行它，直到它返回 **false** ，为了更好地理解这两个函数，我推荐这篇文章[](https://joseph28robinson.medium.com/cs50-pset3-tideman-87f22f0f0bc3)**。**

**在`lock_pairs`中，我们遍历每一对，为每一个输家和赢家调用`cycle`函数。如果`**cycle**` 返回假，那么`locked`数组可以为那个`pair`设置为真**

**`print_winner`函数循环遍历每个候选项，检查它是否被锁定，如果`locked`为假，则增加`falseValues`变量，并检查它是否等于候选项计数，换句话说，它检查是否有一个指向它们的箭头。**

**这是迄今为止最困难的问题，我无法独自解决它，我必须做大量的谷歌搜索并从其他人那里获得帮助，但作为未来的开发人员，我们需要学习这些技能，重要的是我们要向前迈进，因为如果我们在一个问题上停滞不前太久，我们可能会感到沮丧并停止学习，至少这是我以前发生过的事情。 我当时就停止了学习，甚至认为我得到了帮助，我在 3 周内完成了这个问题，我花了很多时间在调试器上，试图理解这里发生的事情和解释，我认为我仍然没有很好地掌握它，但我觉得我需要像我之前说的那样前进。**

**感谢阅读:)。**

**这两篇文章给了我很大的帮助: [art1](https://gist.github.com/nicknapoli82/6c5a1706489e70342e9a0a635ae738c9) ， [art2](https://joseph28robinson.medium.com/cs50-pset3-tideman-87f22f0f0bc3) 。**