# JavaScript 的亮点——字段值、图像和文本

> 原文：<https://blog.devgenius.io/highlights-of-javascript-field-values-images-and-text-95b923379db9?source=collection_archive---------2----------------------->

![](img/bf152c44eaffabe94f2cb190e8769fec.png)

照片由 [D O M I N I K J P W](https://unsplash.com/@dominikjpw?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

学习 JavaScript，必须先学基础。

在本文中，我们将研究 JavaScript 语言的最基本部分。

# 设置字段值

我们可以使用 JavaScript 设置输入字段值。

例如，我们可以编写以下 HTML:

```
<form>
  fruit:<br>
  <input type="text" id="fruit" onBlur="fillColor();"><br>
  color:<br>
  <input type="text" id="color">
</form>
```

然后我们可以编写下面的 JavaScript 代码:

```
const fillColor = () => {
  let color;
  const {
    value
  } = document.getElementById("fruit");
  switch (value) {
    case "apple":
      color = "red";
      break;
    case "orange":
      color = "orange";
      break;
    case "grape":
      color = "purple";
  }
  document.getElementById("color").value = color;
}
```

我们向第一个输入框添加了`onBlur`属性。

因此，当我们将光标从第一个输入移开时，就会调用`fillColor`函数。

如果 ID 为`fruit`的输入的值带有`value`属性，那么我们就会得到这个值。

然后我们根据`value`的值设置`color`变量的值。

然后，我们将 ID 为`color`的输入设置为`color`变量的值。

# 阅读和设置段落文本

我们可以阅读和设置段落文本。

例如，我们可以编写以下 HTML:

```
<p id="text">
  Lorem ipsum dolor sit amet.
  <a href="javascript:void(0);" onClick="expandText();">
    <em>Click for more.</em>
  </a>
</p>
```

然后我们可以添加`expandText`功能如下:

```
const expandText = () => {
  const expandedParagraph = `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras id lorem eget erat vestibulum consectetur. Donec vehicula porta est ut vestibulum. Mauris et nisl a sem iaculis laoreet. Nunc eleifend facilisis massa ut luctus. Nullam sollicitudin non lorem non eleifend. Curabitur elementum felis quis enim malesuada varius.`;
  document.getElementById("text").innerHTML = expandedParagraph;
}
```

`a`元素具有设置为`expandText()`表达式的`onClick`属性。

因此，当我们点击“点击更多”文本时，我们会看到屏幕上显示的扩展文本。

这是因为我们将`expandedParagraph`变量的值设置为 ID 为`text`的元素的`innerHTML`属性的值。

我们可以通过设置`innerHTML`属性向元素中插入任何内容。

例如，我们可以保留现有的 HTML 并将 JavaScript 改为:

```
const expandText = () => {
  const expandedParagraph = `
    <ol>
      <li>Slow</li>
      <li>Fast</li>
      <li>Just-right</li>
     </ol>
   `;
  document.getElementById("text").innerHTML = expandedParagraph;
}
```

我们将 ID 为`text`的元素的内容设置为有序列表。

这就是我们点击“点击了解更多”时看到的内容。

# 操纵图像和文本

我们可以用 JavaScript 操作图像。

例如，我们可以编写以下 HTML:

```
<img src="https://i.picsum.photos/id/23/200/300.jpg?hmac=NFze_vylqSEkX21kuRKSe8pp6Em-4ETfOE-oyLVCvJo" id="image" onClick="makeInvisible();">
```

然后我们可以添加下面的 CSS:

```
.hidden {
  visibility: hidden;
}
```

那么`makeInvisible`的功能是:

```
const makeInvisible = () => {
  document.getElementById("image").className = "hidden";
}
```

`hidden`类的`visibility`属性被设置为`hidden`。

因此，当我们点击图像时，运行`makeInvisible`功能。

然后将`hidden`类添加到`img`元素中。

因此图像将被隐藏。

# 结论

我们可以使用 JavaScript 来设置字段值、操作图像和段落文本。