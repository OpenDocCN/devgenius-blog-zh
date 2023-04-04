# JS 系列#2:掌握 JS 中的数字

> 原文：<https://blog.devgenius.io/master-the-numbers-in-js-2f70b0c0a191?source=collection_archive---------17----------------------->

了解数字类型| JS 基本构建块的所有信息

数字是 JavaScript 中广泛使用的原始数据类型。它用于表示和存储 JavaScript 中的任何数值。

![](img/469a4f51401fe9f4798ac21e232f35a1.png)

# JavaScript 有一种数字类型

许多编程语言有不同的数据类型来存储整数和小数。但是 JavaScript 只有一种类型来存储和表示任何类型的数字。

数字类型可以存储…

1.  正数— (5，+55，35000)
2.  负数— (-55)
3.  整数(整数)—(0，13，33000)
4.  十进制数字— (0.3333，1.2908，-1.678)

## 无效的数字示例

```
let x = 33,000 // unexpected number
let y = $300// A number can't contain comma and $ sign
```

数据类型说明了一种类型可以表示和存储哪些值，它还指定了特定数据类型允许哪些类型的操作。

## 数字的基本运算

```
// Addition
60 + 5 // 65// Subtraction
10 - 2// 8// Multiplication
10 * 6 // 60// Division
30 / 5 // 6
10 / 3 // 3.3333333333333335// Modulo
30 % 4 // 2// Exponent
2**3 // 8
```

在上面的例子中，加法、减法和除法非常简单。但是让我们更关注模和指数。

## 模算子

%被称为模运算符。它返回余数。

```
5 % 2 // 12 % 5 // 2-5 % 2 // -15 % -2 // 1-5 % -2 // -1
```

> 如果像 p % q 那样执行模运算，那么结果的符号总是属于 p。

## 指数算子

指数(**)运算符用于计算幂。如果它被写成`base**exponent`的形式，那么将`base`返回到`exponent`幂。

```
2 ** 3 // 85 ** 2 // 259 ** 0.5 // 3
```

## 表达式评估

看下面这个表情…

5 + 3 * 3 / 3 + 1

简单的表达式很容易计算，但是对于更复杂的表达式，就很难决定应该先执行哪个操作符了。

为了求解一个长表达式，使用了两个经验法则。

1.  ***优先规则***
2.  ***关联规则***

## 优先规则

它说，一些运营商享有高优先级，一些运营商享有低优先级。首先评估享有高优先级的操作员，然后评估低优先级的操作员。

**算术运算符**的优先级表如下，优先级最高的运算符排在前面，优先级最低的运算符排在后面。相同优先级的运算符放在同一行。

1.  指数(**)
2.  乘法(*) /除法(/) /模数(%)
3.  加法(+) /减法(-)

让我们举个例子来了解事实。

```
2 + 3 * 5
= 3 + 15
= 18
```

## 关联规则

一个表达式中有多个相同优先级的操作符。在这种情况下，通常从左到右计算运算符。但是有些运算符有从右到左的关联，必须按从右到左的顺序进行计算。

例如，指数运算符具有从右到左的关联。

看下面的例子来理解关联规则…

```
***Example-1:***2**2**3 
= 2**8 
= 256//Execution starts for right as exponent association is R to L.***Example-2:***3 + 3 * 4 / 3 - 6
= 3 + 12 / 3 - 6
= 3 + 4 - 6
= 7 - 6
= 1// * , / executed before + and -. All operator of same priority are // executed from left to right.
```

## NaN —不是一个数字

讨论数字和数值表达式，还有一个概念是我们现阶段不能离开的。`NaN`的概念。

`NaN` 代表数值，代表非数字的东西。有几个表达式可以生成`NaN`。

让我们来讨论一些`NaN`的表达…

```
0 / 0 //NaN
1 + NaN //NaN
```

> 任何带`NaN`的算术总是领先`NaN`。

## 无穷

还有一个更重要的数值是无穷大，在讨论算术表达式和`Number`类型值的时候必须讨论。

无穷大是一个产生很多大数字的术语。所以我们来看看生成`Infinity`的表达式。

```
1 / 0 // Infinity
-5 / 0 // -Infinity
```

如果你喜欢这篇文章，请关注我:

**中:**[https://medium.com/@maheshshittlani](https://medium.com/@maheshshittlani)
**Github:**[https://github.com/maheshshittlani](https://github.com/maheshshittlani)
**LinkedIn:**[https://in.linkedin.com/in/mahesh-shittlani-638b7429](https://in.linkedin.com/in/mahesh-shittlani-638b7429)