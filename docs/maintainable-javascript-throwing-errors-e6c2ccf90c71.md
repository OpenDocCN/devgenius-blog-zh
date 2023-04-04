# 可维护的 JavaScript —抛出错误

> 原文：<https://blog.devgenius.io/maintainable-javascript-throwing-errors-e6c2ccf90c71?source=collection_archive---------2----------------------->

![](img/e8ce0dda99077917d9870c6892714be8.png)

[瓦西里·科洛达](https://unsplash.com/@napr0tiv?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看抛出错误的方法来了解创建可维护的 JavaScript 代码的基础。

# 错误

当我们的程序遇到一些意外时，我们抛出错误。

可能传递给函数或操作的值不正确。

编程语言定义了规则，当我们偏离规则时，就会抛出错误。

如果不抛出错误并报告给我们，调试几乎是不可能的。

如果一切都悄无声息地失败了，那么要花很长时间才能发现我们的程序出了什么问题。

错误会帮助我们。

我们的问题是错误可能会在意想不到的时间和地点出现。

默认的错误消息可能不会给我们提供足够的信息来解决问题。

为失败做计划总是一个好主意，这样我们就可以在失败发生之前处理它们。

# JavaScript 中抛出错误

为了在 JavaScript 中抛出错误，我们使用带有`Error`实例的`throw`关键字。

例如，我们可以写:

```
throw new Error("an error occurred.")
```

我们传入一条我们希望显示错误的消息。

内置的`Error`类型在所有 JavaScript 实现中都可用。

构造函数接受一个参数，即错误消息。

当一个错误被抛出并且没有被 try-catch 块捕获时，浏览器以浏览器的方式显示消息。

大多数浏览器都有一个显示错误的开发控制台。

我们抛出的错误被视为与我们没有抛出的错误相同。

使用`throw`关键字的一个不好的方法是将它与非`Error`实例一起使用。

例如，我们可以这样写:

```
throw "error";
```

这很糟糕，因为浏览器对此可能不会有同样的反应。

此外，我们丢弃了有用的信息，如堆栈跟踪。

然而，我们可以扔任何我们喜欢的东西。

没有任何东西禁止这样做。

Firefox、Opera 和 Chrome 在抛出时会将非错误对象转换为字符串。

而 IE 没有。

# 投掷失误的优势

抛出我们自己的错误允许我们提供浏览器显示的确切文本。

除了行号和列号，我们还可以包含成功调试错误所需的任何信息。

例如，如果我们有:

```
function getSpans(element) {
  if (element && element.getElementsByTagName) {
    return element.getElementsByTagName("span");
  } else {
    throw new Error("argument not a DOM element");
  }
}
```

如果一个对象不是 DOM 元素，我们就会抛出一个错误。

这样，如果我们看到错误消息，我们就知道没有传入 DOM 元素。

通过这种设置，我们可以看到错误消息，它可以帮助我们在不太费力的情况下调试代码中的错误。

![](img/a92ce4f5eb1e1180c53249b842898b5b.png)

凯利·西克玛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

当我们遇到问题时，抛出错误让我们知道哪里出了问题。

没有它们，当我们的代码做了一些我们没有预料到的事情时，我们就不会知道哪里出了问题。