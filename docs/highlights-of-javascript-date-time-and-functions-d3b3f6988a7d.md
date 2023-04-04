# JavaScript 的亮点—日期、时间和函数

> 原文：<https://blog.devgenius.io/highlights-of-javascript-date-time-and-functions-d3b3f6988a7d?source=collection_archive---------8----------------------->

![](img/7ac5e9b97b2b1fe8700d054da9ab57a5.png)

照片由 [Aron 视觉效果](https://unsplash.com/@aronvisuals?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

学习 JavaScript，必须先学基础。

在本文中，我们将研究 JavaScript 语言的最基本部分。

# 更改日期和时间

`Date`实例让我们改变日期和时间的不同部分。

`setFullYear`让我们设置一个`Date`实例的年份。

`setMonth`让我们设置一个`Date`实例的月份。它接受 0 到 11 之间的数字，其中 0 表示一月，11 表示十二月。

让我们设定一个月中的某一天。

让我们设置一小时后的分钟数。

`setSeconds`让我们设置分秒。

而`setMilliseconds`让我们设置毫秒过秒。

例如，我们可以这样来调用`setFullYear`:

```
let d = new Date();
d.setFullYear(2040);
```

或者:

```
let d = new Date();
d.setMonth(11);
```

剩下的也可以这么叫。

# 功能

函数是我们可以重复使用来做我们想做的事情的代码块。

我们之前用`setFullYear`、`setMonth`等调用函数。

它们可以有 0 个或更多的参数。

例如，我们可以通过编写以下代码来创建自己的`tellTime`函数:

```
function tellTime() {
  let now = new Date();
  let jr = now.getHours();
  let min = now.getMinutes();
  alert(`${hr}:${min}`);
}
```

我们调用`getHours`获取小时数，调用`getMinutes`获取分钟数。

# 将数据传递给函数

函数可以有参数，所以我们可以传递数据给它们。

例如，我们可以写:

```
function greetUser(greeting) {
  alert(greeting);
}
```

`greetUser`方法接受`greeting`参数，我们使用该值作为`alert`函数的参数。

这样，我们可以指定警报的消息。

我们可以通过编写以下代码来调用我们的函数:

```
greetUser('hello james');
```

现在我们应该看到一个显示“hello james”文本的警告。

我们可以用逗号分隔多个参数和自变量。例如，我们可以写:

```
function showMessage(greeting, name) {
  alert(`${greeting}, ${name}`);
}
```

`showMessage`功能有`greeting`和`name`参数。

那么我们可以通过书写来称呼`showMessage`:

```
showMessage('hello', 'mary');
```

有了参数，我们可以自定义函数的调用方式。

# 在函数中返回数据

如果我们想要得到函数计算的数据，那么我们添加`return`语句来从函数返回我们想要的数据。

例如，我们可以写:

```
function getTotal(subtTotal, taxRate) {
  return subTotal * (1 + taxRate);
}
```

我们创建了接受`subTotal`和`taxRate`参数的`getTotal`函数。

然后我们使用`return`语句返回计算结果。

我们可以通过编写以下代码来调用`getTotal`函数:

```
const total = getTotal(100, 0.05);
```

我们传入`subTotal`和`taxRate`的数字，并将返回值赋给`total`。

所以，`total`是 105。

# 结论

我们可以用`Date`实例方法改变日期和时间。

此外，我们可以创建带参数的函数，并在其中返回数据，这样我们就可以在其他地方使用该结果。