# 熊猫系列:从新手到专家——第二部分

> 原文：<https://blog.devgenius.io/pandas-series-from-novice-to-professional-part-02-e7dc0f06ae94?source=collection_archive---------17----------------------->

## 二元运算符、索引和迭代

![](img/eec3d99c75d466e1c91d4dcc5ec8c3c8.png)

在这一部分中，我们将学习如何对序列应用不同的算术和关系运算符，我们还将学习序列数据项的索引和迭代。这是我的熊猫系列的第二部分，如果你想看第一部分，请点击这里。

# 添加系列

如果两个系列的索引匹配，则将两个系列的值相加。`ser1.add(ser2)`和`ser1 + ser2`用于将两个系列相加，如下所示。

如果两个序列的索引不匹配，那么`NaN`将返回。在下面的例子中，`ser1`的索引`one`在`ser2`的索引列表中找不到，而`ser2`的索引`four`在`ser1.`的索引列表中也找不到

我们通过用数字替换`NaN`占位符来添加系列

# 减去级数

我们想从`ser1`中减去`ser2`

*   `ser1.sub(ser2)`从`ser1`中减去`ser2`使得它们的索引匹配，否则返回`NaN`。
*   `ser1.sub(ser2, fill_value = 0)`用`0`填充`NaN`值，然后执行减法运算
*   `ser1.sub(ser2, fill_value = 1)`用`1`填充`NaN`值，然后执行减法运算

# 乘法级数

我们想用`ser2`乘以`ser1`

*   `ser1.mul(ser2)`将`ser1`乘以序列`ser2`的元素，使其索引匹配，否则返回`NaN`。
*   `ser1.mul(ser2, fill_value = 0)`用`0`填充`NaN`值，然后执行乘法运算
*   `ser1.mul(ser2, fill_value = 1)`用`1`填充`NaN`值，然后执行乘法运算

# 划分系列

我们想用`ser2`除`ser1`

*   `ser1.divide(ser2)`将`ser1`除以序列`ser2`的元素，使得它们的索引匹配，否则返回`NaN`。
*   `ser1.divide(ser2, fill_value = 0)`用`0`填充`NaN`值，然后执行除法运算
*   `ser1.divide(ser2, fill_value = 1)`用`1`填充`NaN`值，然后执行除法运算

# 级数的整数除法

*   `ser1.floordiv(ser2)`返回`ser2`除以`ser1`元素的整数除法。

# 级数的模

将`ser1`除以`ser2`并返回余数

*   `%`简单返回`ser1`除以`ser2`后的余数
*   `ser1.mod(ser2, fill_value = 0)`首先，用一个特定的数字填充`NA`值，然后返回`ser1`除以`ser2`后的余数

# 级数的指数幂

返回`ser1`的指数幂，逐元素提升幂`ser2`。

# radd、rsub、rmul、rdiv、rfloor、rmod、rpow

`r`表示反向，`radd`表示反向加法

*   `ser1.add(ser2) = ser1 + ser2`
*   `ser1.radd(ser2) = ser2 + ser1`

与`radd`类似，我们有`rsub, rmul, rdiv, rfloor, rmod`和`rpow`操作

# 联合系列

将序列`ser1`和`ser2`合并，以使结果序列包含来自`ser1`和`ser2`的`max`或`min`值

# 合并第一系列

第一个系列的值将是结果系列的一部分

# 圆形系列

round()方法用于舍入系列数据

# 逻辑运算符

*   `ser1.lt(ser2)`如果`ser1`的元素小于`ser2`的对应元素，则返回`true`
*   如果`ser1`的元素大于`ser2`的对应元素，则`ser1.gt(ser2)`返回`true`
*   `ser1.le(ser2)`如果`ser1`的元素小于或等于`ser2`的对应元素，则返回`true`
*   `ser1.gt(ser2)`如果`ser1`的元素大于或等于`ser2`的对应元素，则返回`true`
*   `ser1.ne(ser2)`如果`ser1`的元素不等于`ser2`的对应元素，则返回`true`
*   `ser1.eq(ser2)`如果`ser1`的元素等于`ser2`的对应元素，则返回`true`

# 两个数列的点积

`@`运算符用于计算并返回两个数列的点积

# 从数据框中访问序列

`frame.get([‘Name’])`用于从数据帧中返回一个序列，`frame.get[[‘Name’, ‘Age’]]`用于返回一个数据帧。

# 从数据框中访问值

## 使用 Series.at()

访问行列对的单个值，如下所示

*   `frame.at[row_number, Column_Name]`用于从指定的行号和列名返回单个值

## 使用 Series.iat()

按整数位置访问行、列对的单个值。

*   `frame.at[row_number, column_number]`用于从指定的行号和列号返回单个值

# 从数据框中访问数据

## 使用 Series.loc()

使用 Series.loc()可以通过标签或布尔数组访问一组行和列。

`.loc[]`主要基于标签，但也可用于布尔数组。

允许的输入有:

*   单个标签，例如`5`或`'a'`
*   标签的列表或数组，例如`['a', 'b', 'c']`
*   带标签的切片对象，例如`'a':'f'`

## 使用 Series.iloc()

`iloc()`是纯基于整数的索引，用于按位置选择。

允许的输入有:

*   整数，例如`5`
*   整数的列表或数组，例如`[4, 3, 0]`
*   带有整数的切片对象，例如`1:7`
*   一个布尔数组，例如`[True, False, True, False]`

# Series.items()

Series.items()返回一个可迭代元组(索引，值)

# Series.pop()

退货并从系列中删除。如果找不到，则引发 KeyError。

# 结论

在本帖中，我们将学习不同的二元运算符，并替换序列中缺失的数据。我们还讨论了系列数据项的索引和迭代。