# 可维护的 JavaScript——包装器和耦合

> 原文：<https://blog.devgenius.io/maintainable-javascript-wrappers-and-coupling-4675dea997a4?source=collection_archive---------2----------------------->

![](img/c09c19cc42471f220d048a6e50aa08cb.png)

由 [Alexander Mils](https://unsplash.com/@alexandermils?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看包装器类型和松散耦合来了解创建可维护 JavaScript 代码的基础。

# 原始包装类型

原始包装器类型是`String`、`Boolean`和`Number`构造函数的实例。

它们是 JavaScript 语言中鲜为人知且经常被误解的部分。

它们都以构造函数的形式存在于全局范围内，并且每一个都代表各自基本类型的对象形式。

它们使原始值表现得像对象一样。

例如，我们可以写:

```
const name = 'james'.toUpperCase()
```

以大写形式返回`'james'`。

这一行所做的是用`String`构造函数将`'james'`转换成一个对象。

然后我们在字符串实例中调用了`toUpperCase`方法。

这就是 JavaScript 引擎在表面下做的事情。

结果返回后，它会被自动销毁。

需要时会创建另一个。

可以在我们自己的代码中使用这些构造函数。

例如，我们可以写:

```
let name = new String("james");
const author = new Boolean(true);
const count = new Number(10);
```

我们使用原始包装器构造函数来创建包装器对象。

但是，它们会使代码变得更长、更混乱，所以我们应该避免使用它们。

谷歌和 Airbnb 风格指南都把它们标了出来。

而像 ESLint 和 JSHint 这样的 linters，如果存在的话都会警告我们。

# UI 层的松散耦合

JavaScript 通常用于创建 web 应用程序 ui。

我们用 HTML 来创建它们，用来定义页面的语义。

CSS 用于页面的样式和布局。

JavaScript 为我们的页面添加了动态行为，使其更具交互性。

CSS 和 JavaScript 就像兄弟姐妹一样。

它不依赖于 CSS，但是我们可以用它来做样式

我们可以有一个只有 HTML 和 CSS 的页面。

有可能有一个只有 HTML 和 JavaScript 的页面。

要使 web UI 代码可维护，需要使各部分的耦合松散。

紧密耦合意味着一个组件直接了解另一个组件，如果一个组件发生变化，那么另一个组件也会发生变化。

例如，如果在我们的 HTML 中到处都使用了`alert` CSS 类，那么当我们需要将`alert`类更改为其他类时，我们需要在其他地方进行更改。

当我们可以对单个组件进行更改而不影响其他组件时，就实现了松耦合。

我们想让代码更改变得容易，所以我们需要组件松散耦合。

然而，两个组件不可能没有耦合。

他们必须以某种方式相互交流。

松散耦合的 web UI 更容易调试。

我们可以查看 HTML 来检查文本或结构。

当样式问题出现时，我们知道它可能出现在 CSS 中。

如果有任何行为问题，我们可以查看 JavaScript。

![](img/ba54ae3acf312e6374ec85cd3efb890c.png)

塔邦·莫克纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们应该保持代码松散耦合，避免在代码中使用原始包装构造函数。