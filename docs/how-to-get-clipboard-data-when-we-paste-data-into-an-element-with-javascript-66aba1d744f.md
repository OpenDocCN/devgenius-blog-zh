# 当我们用 JavaScript 将数据粘贴到一个元素中时，如何获取剪贴板数据？

> 原文：<https://blog.devgenius.io/how-to-get-clipboard-data-when-we-paste-data-into-an-element-with-javascript-66aba1d744f?source=collection_archive---------0----------------------->

![](img/929d97a9f7523aedf9633c06fe7027f4.png)

日本生活方式在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们可能想要获取剪贴板数据，并将其粘贴到 web 应用程序的元素中。

在本文中，我们将研究如何在将剪贴板数据粘贴到 JavaScript 应用程序中时获取它。

# 听粘贴事件

当我们将数据粘贴到一个元素中时，`paste`事件被触发。

因此，当我们自己的事件监听器获取粘贴的内容时，我们可以只监听那个事件。

例如，我们可以编写以下 HTML:

```
<div id='editableDiv' contenteditable='true'>Paste</div>
```

然后我们可以编写下面的 JavaScript 代码来监听`paste`事件:

```
const handlePaste = (e) => {
  let clipboardData, pastedData;
  e.stopPropagation();
  e.preventDefault();
  clipboardData = e.clipboardData || window.clipboardData;
  pastedData = clipboardData.getData('text/plain');
  console.log(pastedData);
}
const div = document.getElementById('editableDiv')
div.addEventListener('paste', handlePaste);
```

我们创建了`handlePaste`函数来获取剪贴板数据。

首先，我们调用`stopPropagation`来停止事件向父元素和祖先元素的传播。

然后我们调用`preventDefault`来防止默认粘贴行为。

然后我们可以从`e.clipboardData`或`window.clipboardData`属性中获取剪贴板数据。

接下来，我们用`'text/plain'`调用`getData`来获取粘贴的文本数据。

然后我们用控制台日志记录`pastedData`。

然后我们用`getElementById`方法选择可编辑的 div。

我们调用 div 上的`addEventListener`来监听`paste`事件。

我们可以通过调用`e.clipboardData.getData`直接获取数据来简化函数。

然后我们可以调用`document.execCommand`对 div 进行粘贴。

为此，我们写道:

```
const handlePaste = (e) => {
  const pastedText = e.clipboardData.getData('text/plain')
  console.log(pastedText)
  document.execCommand('insertText', false, pastedText);
  e.preventDefault();
}const div = document.getElementById('editableDiv')
div.addEventListener('paste', handlePaste);
```

我们得到粘贴的文本:

```
e.clipboardData.getData('text/plain')
```

然后`pastedText`拥有我们粘贴到 div 中的内容。

然后我们运行:

```
document.execCommand('insertText', false, pastedText);
```

用`pastedText`发出`insertText`命令插入文本。

代码的其余部分是相同的。

# 结论

我们可以监听元素上的`paste`事件，并从`paste`事件处理程序的事件对象中获取数据，从而将数据粘贴到元素中。