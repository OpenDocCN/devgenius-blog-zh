# 如何用 JavaScript 在文本区域内渲染 HTML？

> 原文：<https://blog.devgenius.io/how-to-render-html-inside-a-text-area-with-javascript-778d41df00f3?source=collection_archive---------1----------------------->

![](img/42391e9bd9f658ad7a77ea140be5e4e4.png)

由[克里斯·罗西亚克](https://unsplash.com/@oserone?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们希望在文本区域中呈现 HTML。

在本文中，我们将研究如何在文本区域中呈现 HTML 内容。

# 在 contenteditable Div 中呈现内容

HTML 文本区域不能呈现 HTML。

然而，我们可以用`contenteditable`属性编辑 div 的内容。

因此。我们可以使用一个可编辑的 div，就像我们使用一个文本区域一样，但是使用 HTML 内容。

例如，我们可以编写以下 HTML:

```
<div class="editable" contenteditable="true"></div>
<button class="bold">toggle red</button>
<button class="italic">toggle italic</button>
```

然后我们可以用下面的 CSS 样式化它:

```
.editable {
  width: 300px;
  height: 200px;
  border: 1px solid #ccc;
  padding: 5px;
  resize: both;
  overflow: auto;
}
```

然后我们可以得到按钮，当我们点击它们的时候改变文本:

```
const bold = document.querySelector('.bold')
const italic = document.querySelector('.italic')
const editable = document.querySelector('.editable')const toggleRed = () => {
  const text = editable.innerHTML;
  editable.innerHTML = `<p style="color:red">${text}</p>`;
}const toggleItalic = () => {
  const text = editable.innerHTML;
  editable.innerHTML = `<i>${text}</i>`;
}bold.addEventListener('click', toggleRed);
italic.addEventListener('click', toggleItalic);
```

我们将`contenteditable`属性设置为`true`，使 div 可编辑。

我们选择用`document.querySelector`添加的所有元素。

然后我们有了从可编辑的 div 中获取现有的`innerHTML`的`toggleRed`函数。

然后我们添加一个颜色样式设置为红色的`p`元素。

同样，我们用`toggleItalic`函数从可编辑的 div 中获取`innerHTML`。

然后我们在文本周围加上`i`标签。

CSS 只是为可编辑的 div 设置宽度、边框、填充和溢出样式。

现在，当我们单击切换红色和切换斜体时，我们会看到相应的样式应用于我们键入的文本。

# 结论

如果我们想添加一个可以编辑富文本的框，我们可以在一个可编辑的 div 中呈现 HTML，而不是文本区域。