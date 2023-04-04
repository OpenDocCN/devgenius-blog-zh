# 如何用一行普通的 JavaScript 代码轻松设置多种样式？

> 原文：<https://blog.devgenius.io/how-to-set-multiple-styles-easily-with-one-line-of-vanilla-javascript-code-c8f13ba019e6?source=collection_archive---------2----------------------->

![](img/2cfd3b872cce19e2c06bda5537e1cd69.png)

照片由 [Charlie Harutaka](https://unsplash.com/@charlieharutaka?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

用普通的 JavaScript 设置 HTML 元素的多种样式通常需要多行代码。

然而，如果我们将包装对象样式放在一个 JavaScript 代理中，我们可以用一行代码完成所有工作。

在本文中，我们将了解如何将元素对象包装在代理中，以允许设置多种样式。

# 通过将元素包装在代理中，可以轻松设置多种样式

要在代理中包装我们的 HTML 元素对象，我们可以写:

```
const styleProxy = {
  get: (object, property) => {
    return (value) => {
      if (value) {
        object[property] = value;
        return new Proxy(object, styleProxy);
      }
      return object[property];
    }
  }
}const style = (selector) => {
  const element = document.querySelector(selector);
  return new Proxy(element.style, styleProxy);
}style("div")
  .color("#fff")
  .backgroundColor("#000")
  .opacity("1");
```

我们创建了具有`get`方法的`styleProxy`对象，以返回采用样式`value`的函数。

如果我们传入一个`value`，那么我们将它设置为`object`的属性值之一。

然后我们返回代理，用`object`和`styleProxy`作为代理。

否则，我们返回对象。

接下来，我们创建`style`，用带有`querySelector`的给定选择器获取元素，并返回带有`element.style`对象属性和`styleProxy`的代理。

代理让我们获得设置为对象属性值的值，并修改它的设置方式。

在这个例子中，我们返回一个函数，让我们用函数而不是对象本身来设置属性。

因此，我们可以编写以下代码来设置样式:

```
style("div")
  .color("#fff")
  .backgroundColor("#000")
  .opacity("1");
```

我们调用`color`来设置`style.color`。

我们调用`backgroundColor` 来设置`style.backgroundColor`。

我们调用`opacity`来设置`style.opacity`。

# 结论

我们可以通过从选中的 HTML 元素对象创建一个代理，在一行代码中设置多个样式。

在代理中，我们有一个 getter，它返回一个函数，该函数返回一个新的代理来设置其他样式。