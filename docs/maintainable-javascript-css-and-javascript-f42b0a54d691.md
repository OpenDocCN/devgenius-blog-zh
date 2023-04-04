# 可维护的 JavaScript — CSS 和 JavaScript

> 原文：<https://blog.devgenius.io/maintainable-javascript-css-and-javascript-f42b0a54d691?source=collection_archive---------12----------------------->

![](img/6afb7f9c5b9a600cd896221ecee82b85.png)

詹姆斯·贝瑟在 Unsplash[上拍摄的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过研究松散耦合来了解创建可维护的 JavaScript 代码的基础。

# 让 JavaScript 远离 CSS

IE8 允许我们在 CSS 代码中运行 JavaScript 表达式。

这肯定是不好的，因为我们不希望 CSS 和 JavaScript 之间紧密耦合。

我们可以这样写:

```
.box {
  backgroundColor: expression(document.body.backgroundColor);
}
```

这是 IE8 特有的，它引入了紧耦合，因此不应使用。

CSS 经常被重新评估，所以 JavaScript 会频繁运行，所以我们不应该把 JavaScript 代码放在 CSS 中。

这也很难调试，因为 JavaScript 代码在 CSS 文件中，所以没有办法在任何浏览器中跟踪它们。

# 让 CSS 远离 JavaScript

我们可以在任何浏览器中用 JavaScript 设置 CSS 样式。

他们合作得很好。

然而，在 JavaScript 中改变样式意味着必须运行 JavaScript 代码。

所以如果我们有这样的代码:

```
element.style.color = "green";
```

或者

```
element.style.color = "red";
element.style.left = "10px";
element.style.top = "200px";
element.style.visibility = "visible";
```

然后我们必须运行一个或多个操作来改变一个元素的样式。

这也是一个问题，因为如果我们有风格问题，我们不能只是摆弄 CSS 来解决它。

我们必须一步一步地检查 JavaScript 代码，找到问题并修复代码。

这肯定比仅仅打开和关闭 CSS 并改变值要慢。

改变 CSS 的另一个不好的方法是改变元素的`cssText`属性。

例如，有些人可能会写:

```
element.style.cssText = "color: green; left: 10px; top: 100px; visibility: hidden";
```

这也不好，因为样式都在一个字符串中。

因此，我们不能只打开和关闭它们，改变它们的值而不改变代码和刷新。

将 CSS 放在 JavaScript 之外意味着所有的样式信息都在 CSS 中。

所以我们写了这样的东西:

```
.box {
  color: red;
  left: 10px;
  top: 100px;
  visibility: visible;
}
```

在我们的 CSS 文件中。

这样，我们可以在浏览器开发控制台中摆弄它们，而不是更改代码和刷新。

此外，由于 CSS 是自包含的，所以耦合更松散。

在我们的 JavaScript 代码中，我们可以通过编写以下代码来打开该类:

```
element.className += " box";
```

或者:

```
element.classList.add("box");
```

我们可以通过以下方式删除它们:

```
element.classList.remove("box");
```

CSS 类名是 CSS 和 JavaScript 之间的通信机制。

JavaScript 用于在页面的整个生命周期中添加和删除元素的类名。

样式由 CSS 代码中的类应用。

现在我们只需要改变 CSS，而不是直接改变操作风格。

![](img/636b9a6d4f009bd6332bf30280b92416.png)

照片由[卡西迪·詹姆斯·布拉德](https://unsplash.com/@cassidyjames?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们应该让 JavaScript 远离 CSS，让 CSS 远离 JavaScript。

这样，我们就不会有杂乱的代码与难以调试的代码紧密耦合。