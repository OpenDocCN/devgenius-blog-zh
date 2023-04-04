# 如何用 JavaScript 检测文本输入字段中 Enter 键的按下？

> 原文：<https://blog.devgenius.io/how-to-detect-the-pressing-of-the-enter-key-in-a-text-input-field-with-javascript-380fb2be2b9e?source=collection_archive---------1----------------------->

![](img/15d3ae8ab7b5e67efb569e5f8f703fa8.png)

由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们希望在代码中检测到 enter 键的按下，然后在这种情况发生时做一些事情。

在本文中，我们将看看如何用 JavaScript 来检测文本输入字段中 enter 键的按下。

# 检查 event.key 属性

检查哪个键被按下的一种方法是使用`event.key`属性。

例如，我们可以编写以下 HTML:

```
<input />
```

和下面的 JavaScript 代码来检测 Enter 键是否被按下:

```
const input = document.querySelector("input");
input.addEventListener("keyup", (event) => {
  if (event.key === "Enter") {
    console.log('Enter key pressed')
  }
});
```

我们用`document.querySelector`方法得到输入元素。

然后我们调用`addEventListener`为`keyup`事件添加一个事件监听器。

然后在事件处理程序中，我们检查`event.key`属性是否返回`'Enter'`。

如果是，则按回车键。

# 检查 event.keyCode 属性

我们可以检查的另一个属性是`event.keyCode`属性。

属性返回被按下的键的数字，而不是带有键名的字符串。

当它返回 13 时，我们知道 enter 键被按下了。

所以我们可以写:

```
const input = document.querySelector("input");
input.addEventListener("keyup", (event) => {
  if (event.keyCode === 13) {
    console.log('Enter key pressed')
  }
});
```

检查是否按下了键代码为 13 的键。

如果是，那么我们知道 enter 键被按下了。

# 检查事件。哪个属性

像`event.keyCode`属性一样，`event.which`属性也返回被按下的键的键码。

例如，我们可以写:

```
const input = document.querySelector("input");
input.addEventListener("keyup", (event) => {
  if (event.which === 13) {
    console.log('Enter key pressed')
  }
});
```

用`event.which`做同样的检查。

其他的都应该和检查`event.keyCode`属性一样。

# 结论

输入事件对象有几个属性，我们可以检查哪个键被按下，以检查 enter 键是否被按下。