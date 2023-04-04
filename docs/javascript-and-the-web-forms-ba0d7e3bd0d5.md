# JavaScript 和网络表单

> 原文：<https://blog.devgenius.io/javascript-and-the-web-forms-ba0d7e3bd0d5?source=collection_archive---------36----------------------->

![](img/f70fc1859616675f72475f4c513695cf.png)

由 [Severin Stalder](https://unsplash.com/@severinstalder?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将了解如何使用表单。

# 焦点

表单域可以获得键盘焦点。

当我们以某种方式单击或激活它时，它们就成为当前活动的元素，并接收键盘输入。

当它被聚焦时，我们可以在文本字段中输入内容。

其他字段对键盘事件的响应不同。

`select`菜单试图移动到包含用户输入文本的选项。

他们也通过上下移动选择来响应箭头键。

我们可以用`focus`和`blur`方法聚焦一个输入元素。

例如，我们可以写:

```
document.querySelector("input").focus();
```

然后`document.activeElement.tagName`变为`'INPUT'`。

如果我们叫`blur`:

```
document.querySelector("input").blur();
```

然后`document.activeElement.tagName`变为`'BODY'`。

HTML 还提供了`autofocus`属性来产生聚焦输入的相同效果。

`tabinddex`属性让我们设置何时通过按 Tab 键到达元素。

如果`tabindex`为 1，那么第一个 Tab 键就聚焦在那个元素上。

添加`tabindex`可以使任何 HTML 元素成为焦点。

`tabindex` of -1 使跳转跳过一个元素。

# 禁用字段

可以通过`disabled`属性禁用表单字段。

例如，我们可以将它添加到一个按钮上:

```
<button disabled>disabled</button>
```

则禁用按钮将被禁用。

# 形状要素

如果存在一个`form`元素，那么`document`对象有一个`forms`属性链接到页面上的表单。

表单具有`elements`属性，其中包含表单内部的元素。

例如，如果我们有:

```
<form>
  <input type='text'>
</form>
```

然后当我们写下:

```
const form = document.querySelector('form');
console.log(form.elements[0].type);
```

我们得到`'text'`日志。

`form.elements[0]`是输入字段本身。

如果输入的`type`为`submit`，那么当输入按钮被按下时，表单将被提交。

如何由`action`属性提交。这可以通过 GET 或 POST 请求来完成。

在此之前，触发了`submit`事件。

我们可以通过在提交事件处理程序中调用`preventDefault`来阻止默认的提交行为。

如果我们有下面的 HTML:

```
<form>
  <input type='text'>
  <input type='submit'>
</form>
```

我们可以写:

```
const form = document.querySelector('form');
form.addEventListener("submit", event => {
  event.preventDefault();
  console.log(form.elements[0].value);
});
```

我们可能希望拦截 JavaScript 中的提交事件，以便在提交之前检查表单值的有效性。

此外，我们可能希望通过`fetch`提交它，而不需要重新加载页面。

# 文本字段

`type`设置为`text`或`password`的文本区或输入共用一个接口。

它们都有将输入值作为字符串的`value`属性。

文本字段的`selectionStart`和`selectionEnd`属性为我们提供了关于光标和文本选择的信息。

当未选择任何内容时，它们返回相同的数字。

0 表示文本的开始，1 是第二个字符，依此类推。

`replaceSelection`功能用给定的单词替换当前选中的文本部分，并将光标移到该单词之后，以便用户可以继续输入。

文本字段的`change`事件不会在每次输入内容时触发。

它在内容更改后失去焦点时触发。

为了监听所有输入值的变化，我们应该监听`input`事件。

![](img/19ba8f585c929ef93f1fa6a5275fe375.png)

照片由[穆罕默德·穆扎米尔](https://unsplash.com/@gr8muzamil?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 复选框和单选按钮

为了监听复选框的变化，我们可以使用`checked`属性。

例如，如果我们有下面的 HTML:

```
<input type="checkbox" id="green">
```

我们可以写:

```
const checkbox = document.querySelector("#green");
checkbox.addEventListener("change", () => {
  document.body.style.background =
    checkbox.checked ? "green" : "";
});
```

我们使用了`checked`属性来检查复选框的值。

勾选的话就是`true`了。

当 body 元素被选中时，我们将它的背景颜色设置为`'green'`。

label 标签将一段文档与一个输入字段相关联。

例如，我们可以写:

```
<label>
  <input type="radio" name="fruit" value="orange"> Orange
</label>
<label>
  <input type="radio" name="fruit" value="banana"> Banana
</label>
<p id='fruit'></p>
```

我们可以按如下方式检查单选按钮的:

```
const buttons = document.querySelectorAll("[name=fruit]");
for (let button of Array.from(buttons)) {
  button.addEventListener("change", () => {
    document.querySelector('#fruit').innerText = button.value;
  });
}
```

然后，当我们单击一个单选按钮时，我们会看到在 p 元素中填充了 select 选项。

# 结论

我们可以通过不同的方式获得单选按钮、复选框和文本输入的值。

我们可以监听 change 事件来获取输入控件模糊后的值。