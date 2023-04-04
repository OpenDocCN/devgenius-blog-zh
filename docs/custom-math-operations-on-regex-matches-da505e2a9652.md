# 正则表达式匹配的自定义数学运算

> 原文：<https://blog.devgenius.io/custom-math-operations-on-regex-matches-da505e2a9652?source=collection_archive---------11----------------------->

## 使用 Python 正则表达式计算文本中的数字

本文使用正则表达式对文本中的数字执行数学运算。作为一个例子，我们将使用一个食谱。我们将按比例增加配料的数量。我们还将介绍函数闭包的概念。最后，我们将把这些操作应用到一个熊猫数据帧上。

![](img/602122cd5f2b7d130f6a0817b8871e89.png)

詹姆斯·哈里逊在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 在 re1 中使用 lambda 函数

```
 recipe = """
100g plain flour
2 large eggs
300ml milk
"""
```

比方说，你想将一份食谱中的所有配料进行缩放，这样就能养活 5 倍多的人。我们使用 re.sub 方法找到我们的模式，并用我们的计算结果替换它。

```
re.sub("(\d+)(?=.+\n)", lambda x: str(int(x.group())*5), recipe)
```

我们捕获一组数字，后面跟着一个前瞻断言，声明必须出现一个新行。如果你想了解更多关于正则表达式组和断言的信息，你可以阅读[我以前的文章](/advanced-use-of-regex-groups-147ebfcbb139)。

## 创建函数闭包以动态确定乘数

我们再来看看另一种情况。我们知道我们需要为一定数量的人提供食物，食谱上注明了它能为多少人提供食物。我们想要调整所有的成分以符合我们想要的数量。

```
recipe = """
For 3 people:
100g plain flour
2 large eggs
300ml milk
"""
```

对于这个任务，我们将使用**闭包**。它们是返回预先设置了一些参数的函数。我们需要一个类似于上例的函数，但是我们需要动态地推断乘数。re.sub 函数不允许向函数发送多个属性，因此闭包是必要的。

```
def multiply_constructor(multiplier):
    def multiply(x):
        return str(int(x.group()) * multiplier)
    return multiply
```

我们的闭包为 multiplier 生成了一个带有设定值的新函数。我们将使用缩放食谱所需的乘数来构造内部函数。

首先，我们的文本有点不同，它需要一个变化来区分配料和食谱喂养的人数。配料之前总是有一个新行。我们将使用[回顾断言](/advanced-use-of-regex-groups-147ebfcbb139)来满足这一要求。

```
desired_quantity = 9

m = re.search("(?<=For )(\d+)", recipe)
recipe_quantity = int(m.group())

recipe = re.sub("(?<=For )(\d+)", f"{desired_quantity}" ,recipe)

multiply = multiply_constructor(int(desired_quantity/recipe_quantity))
recipe = re.sub("(?<=\n)(\d+)(?=.+\n)", multiply, recipe)
```

我们希望我们的食谱能养活 9 个人。我们的第一个正则表达式搜索当前数量，因此我们的乘数是我们想要的数量除以当前配方中的数量。

这是我们修改过的食谱:

```
For 9 people:
300g plain flour
6 large eggs
900ml milk
```

## 在熊猫数据帧上应用正则表达式替换

让我们看看如何将我们的解决方案扩展到一个 **pandas 数据帧**中的多个文本。我们将在熊猫[应用函数](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html)中使用我们之前定义的闭包。

这是我们的示例数据框:

```
import pandas as pd

df = pd.DataFrame({
    "desired_number_of_people":[9, 9, 9],
    "number_of_people":[3, 2, 4],
    "text": [
        "\n100g plain flour\n2 large eggs\n300ml milk\n", 
        "\n500g strong white flour\n2 tsp salt\n300ml water\n",
        "\n2 tbsp olive oil\n2 onions\n1kg pumpkin\n150ml double cream\n"
    ] 
})
```

我们将使用前两列中的值来确定乘数。我们将在 apply 函数中构造新函数。

```
df["text"] = (
    df.apply(lambda x: re.sub(
        "(?<=\n)(\d+)(?=.+\n)",
        multiply_constructor(x["desired_number_of_people"]/x["number_of_people"]),
        x["text"]
    ), axis=1)
)
```

我们必须指定我们正在轴 1(列)上工作。我们修改后的食谱是这样的:

```
300.0g plain flour
6.0 large eggs
900.0ml milk

2250.0g strong white flour
9.0 tsp salt
1350.0ml water

4.5 tbsp olive oil
4.5 onions
2.25kg pumpkin
337.5ml double cream
```

## 结论

我们希望这个故事能帮助你更好地理解正则表达式的用法。所有的代码都可以在 [Github](https://github.com/BorutFlis/Scripts/blob/main/custom_math_operations.py) 上获得。

谢谢你读了这个故事。