# 可维护的 JavaScript —错误类和检查

> 原文：<https://blog.devgenius.io/maintainable-javascript-error-class-and-checks-880cb0f6571f?source=collection_archive---------4----------------------->

![](img/c8fd731425fa424b120c624fcc01f97a.png)

照片由 [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看处理错误的最佳方式来了解创建可维护的 JavaScript 代码的基础。

# 试抓

我们不应该有空块的`try-catch`语句。

应该以某种方式处理它们，这样我们的代码就不会无声无息地失败。

例如，我们不写:

```
try {
  errorFunc();
} catch (ex) {
  // do nothing
}
```

我们不想忽略错误。

# 错误类型

ES 规范指定了几种内置错误类型。

`Error`是所有错误的基本构造函数。

`EvalError`在`eval`执行过程中出现错误时抛出。

`RangeError`当一个数超出其范围的界限时抛出。

它们很少在正常执行过程中出现。

`ReferenceError`当需要一个对象但它不可用时抛出。

当代码有语法错误时抛出。

当变量属于意外类型时抛出。

当我们传递给`encodeURI`、`encodeURIComponent`、`decodeURI`或`decodeURIComponrnt`的 URI 格式不正确时，抛出`URIError`。

如果我们了解不同类型的错误，那么处理它们就很容易了。

所有错误类型都继承自 error。

如果我们检查特定类型的错误，我们会得到更好的错误处理。

例如，我们可以写:

```
try {
  // ...
} catch (ex) {
  if (ex instanceof TypeError) {
    // ...
  } else if (ex instanceof ReferenceError) {
    // ...
  } else {
    // handle all others
  }
}
```

我们用`instanceof`操作符检查抛出了哪种类型的错误，并且我们可以为每种错误编写错误处理代码。

投掷`Error`实例有其优势。

`Error`抛出的对象由浏览器的错误处理机制处理。

在`Error`对象中还有额外的信息，如行号、列号和堆栈跟踪。

如果我们想抛出自定义错误，我们可以创建自己的`Error`类。

例如，我们可以写:

```
class MyError extends Error {
  constructor(message) {
    super();
    this.message = message;
  }
}
```

我们使用了`extends`关键字来创建`Error`的子类。

然后我们可以通过写来使用它:

```
throw new MyError('error occurred');
```

现在，如果我们抛出错误，那么它将像任何本机错误一样响应。

大多数现代浏览器都会以同样的方式处理它们，所以我们不必担心浏览器的兼容性。

使用我们自己的错误类，我们可以通过编写以下代码来检查错误类型:

```
try {
  // ...
} catch (ex) {
  if (ex instanceof MyError) {
    // ...
  } else {
    // handle all others
  }
}
```

我们用`instanceof`检查错误类型，就像检查其他错误一样。

![](img/d788a4a7c69edd1068201fa85eee931f.png)

由 [Philip Kock](https://unsplash.com/@philipkck?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以创建自己的错误类，以便在错误中包含我们想要的信息。

此外，我们检查用`instanceof`操作符抛出的错误类型。

JavaScript 有几种内置的错误类型。