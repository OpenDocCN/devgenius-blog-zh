# JavaScript 技巧——随机性和数组

> 原文：<https://blog.devgenius.io/javascript-tips-randomness-and-arrays-226526764f5e?source=collection_archive---------8----------------------->

![](img/b21f5231714709f92df163c596a86bd0.png)

照片由[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些在编写 JavaScript 代码时应该遵循的技巧。

# **获取特定范围内的随机数**

我们可以用`Math.random()`方法从指定的范围内得到一个随机数。

例如，我们可以写:

```
const min = 2;
condt max = 10;
const x = Math.floor(Math.random() * (max - min + 1)) + min;
```

因为`Math.random()`只返回一个 0 到 1 之间的数字，我们必须用`max — min + 1`乘以它返回的数字。

然后我们把它加到`min`上，使它在范围内。

# **生成一个从 0 到最大的数字数组**

我们可以通过使用 for 循环来创建一个数字数组。

例如，我们可以写:

```
let numbersArray = [] , max = 100;for (let i = 1; numbersArray.push(i++) < max);
```

我们使用`for`循环在每次迭代中使`i`增加 1。

然后我们以到达`max — 1`结束。

`i++`递增并返回我们返回的数字。

# **生成一组随机的字母数字字符**

我们可以使用`toString`和`substr`方法创建一组随机的字母数字字符。

例如，我们可以写:

```
function generateRandomAlphaNum(len) {
  let randomString = "";
  while (randomString.length < len) {
    randomString += Math.random()
      .toString(36)
      .substr(2);
  }
  return randomString.substr(0, len);
}
```

我们有`generateRandomAlphaNum`功能。

我们使用`while`循环来生成字符串，直到给定的`len`值。

为了生成角色，我们使用带有`toString`和`substr`调用的`Math.random()`来创建角色。

# **混洗一组数字**

我们可以用`sort`方法洗牌。

例如，我们可以写:

```
const numbers = [5, 2, 4, 3, 6, 2];
numbers = numbers.sort(() => { return Math.random() - 0.5});
```

我们返回一个正的或负的随机数，这样只有当它是正数时，顺序才会改变。

这是随机的，所以我们洗牌。

# **一个字符串修剪功能**

我们可以用`replace`方法和一些正则表达式来修剪所有的空白。

例如，我们可以写:

```
str.replace(/^s+|s+$/g, "")
```

我们用正则表达式修剪所有的空白。

JavaScript 字符串也有`trim`方法来进行修整。

# **将一个数组追加到另一个数组**

要将一个数组附加到另一个数组，我们可以使用 spread 操作符和`push`。

例如，我们可以写:

```
array1.push(...array2);
```

我们也可以在没有`push`的情况下使用 spread 运算符:

```
const arr = [...arr1, ...arr2];
```

# **将一个** `**rguments**` **转换成一个数组**

我们可以用 rest 操作符获得数组形式的参数。

例如，如果我们有:

```
const foo = (...args) => {}
```

那么无论我们在`args`中有什么，都将是一个数组。

我们可以将任何东西传入`foo`，它将在`args`结束。

![](img/72fe487b17783118831fd7409b1a16ef.png)

照片由 [Riho Kroll](https://unsplash.com/@rihok?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用`Math.random`方法生成随机实体。

此外，我们可以向数组追加项并获取参数。