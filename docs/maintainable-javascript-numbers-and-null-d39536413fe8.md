# 可维护的 JavaScript —数字和空值

> 原文：<https://blog.devgenius.io/maintainable-javascript-numbers-and-null-d39536413fe8?source=collection_archive---------5----------------------->

![](img/52df3435deef5469949b24561b431df7.png)

斯文·布兰德斯马在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过一些数字和`null`的约定来了解创建可维护的 JavaScript 代码的基础。

# 数字

JavaScript 中只有一种数字。

整数和浮点数以相同的数据类型存储。

我们可以写各种各样的数字文字。

例如，我们可以写:

```
const count = 10;
```

写一个整数。

要写小数，我们可以写:

```
let price = 10.0;
var quantity = 10.00;
```

数字后面可以有小数。

然而，我们也可以写:

```
const count = 10.;
```

但是这是令人困惑的，所以我们应该避免它。

挂小数也没用。

我们也可以在 JavaScript 数字前加一个小数点:

```
var price = .2;
```

但是把 0 放在小数点前面会更清楚:

```
var price = 0.2;
```

我们不应该写八进制文字，因为它们容易混淆，不推荐使用:

```
var num = 010;
```

0 导致八进制数和十进制数混淆。

我们也可以写 JavaScript 十六进制数:

```
let num = 0xFF;
```

要用科学记数法写数字，我们可以使用字母`e`:

```
var num = 1e20;
```

`1e20`是`100000000000000000000`还是`10 ** 20`。

悬挂小数和前导小数很容易被误认为是错误的。

它们在许多样式指南中是被禁止的，但可以用 ESLint、JSLint 和 JSHint 来捕捉。

如果遇到八进制文字，也会发出警告。

# 空

`null`常被误解，与`undefined`混淆。

我们大部分时间应该使用`undefined`来减少混乱。

但是我们可以在少数情况下使用`null`。

我们可以用它们来初始化一个变量，这个变量以后可能会被赋值给一个对象。

此外，我们可以用它来与一个可能是`null`的变量进行比较。

我们可以把它传递给一个函数，在这个函数中应该有一个对象。

当没有东西可返回时，我们也可以返回`null`来代替一个对象。

但是我们不应该使用`null`来测试一个参数是否被提供。

我们不会测试`null`的未初始化变量。

所以我们可以写:

```
let person = null;
```

或者我们可以写:

```
function createPerson() {
  if (condition) {
    return new Person("nick");
  } else {
    return null;
  }
}
```

但是我们不应该用它来比较未初始化的变量，比如:

```
if (person != null) {
  doWork();
}
```

我们也不应该检查`null`来查看变量是否被传入:`

```
function doWork(arg1, arg2, arg3, arg4) {
  if (arg4 != null) {
    doSomething();
  }
}
```

我们使用了`!=`,这是不好的，因为它进行自动数据类型强制，当我们应该检查`undefined`时，我们检查了`null`。

这是因为如果我们不传入一个参数，那么参数将是`undefined`。

`null`是物体的占位符。

什么都不代表不是一个值。

![](img/7611f6d014090ee52e97ccb45c64cdac.png)

本·赫尔希在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们应该小心使用数字的小数位。

此外，我们不应该使用八进制文字。

`null`应该只在一些有限的情况下使用。