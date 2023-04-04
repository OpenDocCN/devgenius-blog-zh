# 如何用 JavaScript 设置 contentEditable Div 上的光标位置？

> 原文：<https://blog.devgenius.io/how-to-set-the-cursor-position-on-contenteditable-div-with-javascript-1bfe86d5fae4?source=collection_archive---------0----------------------->

![](img/ae5496c2945b61c06972696d7a40b0c5.png)

安妮·尼加德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们想用 JavaScript 在一个`contenteditable` div 上设置光标位置。

在本文中，我们将看看如何用 JavaScript 在一个`contenteditable` div 上设置光标位置。

# 使用 document.creatRange 方法

我们可以使用`document.createRange`方法来创建选择范围。

然后我们可以在返回的选择对象上调用`addRange`来选择内容。

例如，如果我们有下面的 HTML:

```
<div id="selectable" contenteditable>http://example.com/page.htm</div>
```

然后我们可以在上面写下:

```
const el = document.getElementById('selectable');
const selection = window.getSelection();
const range = document.createRange();
selection.removeAllRanges();
range.selectNodeContents(el);
range.collapse(false);
selection.addRange(range);
el.focus();
```

我们用`getElementById`得到 div。

然后我们调用`window.getSelection`来创建一个选择。

然后我们调用`document.createRange`来创建选择范围。

接下来，我们调用`removeAllRanges`来删除任何现有的选择。

然后我们用`el`调用`selectNodeContents`将选择应用到 div。

然后我们调用`addRange`来选择内容。

最后，我们调用`el.focus()`来关注添加光标的元素。

现在我们应该看到光标在文本的末尾。

# 结论

我们可以用 JavaScript browser `document.createRange`方法在`contenteditable` div 上设置光标位置。