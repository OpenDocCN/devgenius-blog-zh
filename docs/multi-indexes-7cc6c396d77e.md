# 多索引

> 原文：<https://blog.devgenius.io/multi-indexes-7cc6c396d77e?source=collection_archive---------18----------------------->

## 引用

## *摘自鲁文·勒纳的* [*熊猫健身*](https://www.manning.com/books/pandas-workout?utm_source=medium&utm_medium=referral&utm_campaign=book_lerner2_pandas_8_3_21)

*![](img/01b99a8912d3731a3d7116ab4f67c10e.png)*

*[m.](https://unsplash.com/@m_____me?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/index?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照*

**本文讨论在熊猫中使用多指标。**

*在[manning.com](https://www.manning.com/books/pandas-workout?utm_source=medium&utm_medium=referral&utm_campaign=book_lerner2_pandas_8_3_21)的结账处，将 **fcclerner2** 输入折扣代码框，即可享受[*熊猫健身*](https://www.manning.com/books/pandas-workout?utm_source=medium&utm_medium=referral&utm_campaign=book_lerner2_pandas_8_3_21) 七五折优惠。*

## ***陈述 SAT 成绩(学习目标:设置和使用多指标)***

*设置索引可以让我们更容易地创建关于数据的查询。但有时我们的数据本质上是分层的。这就是“多指数”概念发挥作用的地方。使用多索引，您不仅可以将索引设置为单个列，还可以设置为多个列。例如，设想一个包含销售数据的数据框:您可能希望按年份细分销售，然后再按地区细分。一旦你使用“进一步分解”这个短语，多指数几乎肯定是一个好主意。*

*在这篇文章中，我们将查看 SAT(美国广泛使用的标准化大学入学考试)的分数汇总，并与考生的大学平均绩点(也称为 GPA，4.0 是最高分)进行比较——尽管我们暂时忽略 GPA。CSV 文件(`sat-scores.csv`)有 99 列和 577 行，描述了美国所有 50 个州和三个非州(波多黎各、维尔京群岛和 DC 华盛顿州)，从 2005 年到 2015 年。*

*在本练习中，我希望您:*

*   *读入成绩文件，只保留`Year`、`State.Code`、`Total.Math`、`Total.Test-takers`、`Total.Verbal`列。*
*   *基于年份和两个字母的州代码创建多索引。*
*   *2005 年总共有多少人参加了 SAT 考试？*
*   *纽约州(`NY`)、新泽西州(`NJ`)、马萨诸塞州(`MA`)和伊利诺伊州(`IL`)2010 年 SAT 数学平均成绩是多少？*
*   *亚利桑那州(`AZ`)、加利福尼亚州(`CA`)和得克萨斯州(`TX`)2012 年至 2015 年的 SAT 平均成绩是多少？*

## *讨论*

*本文将展示多索引的强大功能和灵活性。首先，您需要加载一个 CSV 文件并创建一个基于“年份”和“州”的多索引。代码”列。我们可以分两个阶段完成这项工作，首先将文件(包括我们需要的列)读入数据帧，然后选择两列作为我们的索引:*

```
*filename = '../data/sat-scores.csv'

 df = pd.read_csv(filename,
                 usecols=['Year', 'State.Code', 'Total.Math', 'Total.Test-takers', 'Total.Verbal'])
 df = df.set_index(['Year', 'State.Code'])*
```

*注意，和往常一样，`set_index`的结果是一个新的数据帧，我们将它赋回给`df`。*

*然而，您可能记得`read_csv`也有一个`index_col`参数。如果我们将一个参数传递给那个参数，那么我们可以告诉`read_csv`在一个步骤中完成所有的工作——读入数据帧，并将索引设置为我们请求的列。然而，我们可以将一列列表作为参数传递给`index_col`，从而在收集数据帧时创建多索引。例如:*

```
*filename = '../data/sat-scores.csv'

 df = pd.read_csv(filename,
                 usecols=['Year', 'State.Code', 'Total.Math', 'Total.Test-takers', 'Total.Verbal'],
                 index_col=['Year', 'State.Code'])*
```

*现在我们已经加载了数据框，我们可以开始研究数据并回答一些问题。*

*首先，我想知道 2010 年四个州——纽约、新泽西、马萨诸塞州和伊利诺伊州——学生的平均数学成绩。像往常一样，我们希望使用`loc`来检索我们感兴趣的数据。但是我们需要结合三件事来创建正确的查询:*

*   *从多指数的第一部分(`Year`)来看，我们只要 2010 年。*
*   *从多指标的第二部分(`State.Code`)我们只要`NY`、`NJ`、`MA`、`IL`。*
*   *从栏目来看，我们感兴趣的是`Total.Math`。*

*请记住，当我们从多索引中检索时，我们需要将各个部分放在一个元组中。此外，我们可以通过使用列表来表明我们想要多个值。结果是:*

```
*df.loc[(2010, ['NY', 'NJ', 'MA', 'IL']), 'Total.Math'].mean()*
```

*上面的查询检索 2010 年的行，并且来自这四个州中的任何一个。我们只得到`Total.Math`列，然后在其上计算平均值。*

*下一个问题要求类似的计算，但是是几年，以及几个州。同样，如果我们仔细考虑如何构造查询，这不是问题:*

*   *从多指数的第一部分(`Year`)我们要的是 2012 年，2013 年，2014 年，2015 年。*
*   *从多指标的第二部分(`State.Code`)，我们要`AZ`、`CA`、`TX`。*
*   *从专栏中，我们再次对`Total.Math`感兴趣。*

*然后，查询变成:*

```
*df.loc[([2012,2013,2014,2015], ['AZ', 'CA', 'TX']), 'Total.Math'].mean()*
```

*请注意`pandas`是如何计算出如何组合我们的多索引的各个部分的，这样我们只得到匹配两个部分的行。*

## *解决办法*

```
*filename = '../data/sat-scores.csv'

 df = pd.read_csv(filename,
                 usecols=['Year', 'State.Code', 'Total.Math', 'Total.Test-takers', 'Total.Verbal'])

 df = df.set_index(['Year', 'State.Code']) ❶
 df.loc[2005, 'Total.Test-takers'].sum() ❷
 df.loc[(2010, ['NY', 'NJ', 'MA', 'IL']), 'Total.Math'].mean() ❸
 df.loc[([2012,2013,2014,2015], ['AZ', 'CA', 'TX']), 'Total.Math'].mean()❹*
```

*❶将指数设为`Year`和`State.code`的组合*

*❷检索包含 2005 的行和列`Total.Test-takers`，然后对这些值求和*

*❸检索 2010 年和这四个州中任何一个州的行，以及列`Total.Math`，然后得到平均值*

*❹检索 2012-2015 年这三个州的行和列`Total.Math`，然后获得平均值*

## *超越练习*

*   *佛罗里达州、印第安纳州和爱达荷州历年的平均数学和语言成绩是多少？(你没有按州来划分这些值。)*
*   *哪一个州获得了最高的口语分数，在哪一年？*
*   *2005 年的数学平均分比 2015 年高还是低？*

## ***按索引排序***

*当我们在`pandas`中谈论数据排序时，我们通常指的是数据排序。例如，我可能希望数据框中的行按价格或地区销售代码排序。*

*但是`pandas`让我们以一种额外的方式对数据帧进行排序:基于索引值。我们可以使用方法`sort_index`来实现这一点，像许多其他方法一样，该方法返回一个具有相同内容的新数据框，但是它的行是基于索引值排序的。我们可以这样说:*

```
*df = df.sort_index()*
```

*如果您的数据框包含多索引，那么排序将首先沿着第一级排序，然后沿着第二级排序，依此类推。*

*除了一些美学上的好处之外，按索引对数据框进行排序可以使某些任务变得更容易，甚至是可能的。例如，如果你试图选择一片行，`pandas`坚持索引将被排序，以避免歧义。*

*如果数据框未排序且具有多索引，则执行某些操作可能会导致警告:*

```
*PerformanceWarning: indexing past lexsort depth may impact performance*
```

*这是`pandas`试图告诉你，大尺寸、多索引和无序索引的组合可能会给你带来麻烦。您可以通过索引对数据框进行排序来避免该警告。*

*如果要检查数据框是否已排序，可检查此属性:*

```
*df.index.is_monotonic_increasing*
```

*注意，这是**而不是**一个方法，而是一个布尔值。还要注意，一些老的文档和博客提到了方法`is_lexsorted`，在`pandas`的最新版本中已经被弃用；你应该使用`is_monotonic_increasing`。*

*本文到此为止。如果你想了解更多，请点击这里查看这本书。*