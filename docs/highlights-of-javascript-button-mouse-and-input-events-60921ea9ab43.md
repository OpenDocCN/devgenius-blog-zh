# JavaScript 的亮点——按钮、鼠标和输入事件

> 原文：<https://blog.devgenius.io/highlights-of-javascript-button-mouse-and-input-events-60921ea9ab43?source=collection_archive---------11----------------------->

![](img/69571264011046aaf5b9d7956d9a8c36.png)

塞缪尔·佩雷拉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

学习 JavaScript，必须先学基础。

在本文中，我们将研究 JavaScript 语言的最基本部分。

# 按钮事件

我们可以在按钮上添加事件处理代码，这样当我们点击它时，它就可以做一些事情。

例如，我们可以写:

```
<input type="button" value="Click Me" onClick="alert('Hello world!');">
```

我们有一个按钮，它有一个带有 JavaScript 代码的`onClick`属性来调用`alert`。

因此，当我们单击它时，我们会看到一个警告框。

`value`属性有按钮文本。

我们可以对其他元素做同样的事情。

例如，我们可以写:

```
<a href="http://google.com"><img onClick="alert('Hello world!');" src="https://i.picsum.photos/id/23/200/300.jpg?hmac=NFze_vylqSEkX21kuRKSe8pp6Em-4ETfOE-oyLVCvJo"></a>
```

当我们点击图像时，我们看到显示的警告框。

如果我们想让代码更干净，我们可以把`alert`调用放到它自己的函数中。

我们可以编写下面的 HTML:

```
<a href="http://google.com"><img onClick="greet()" src="https://i.picsum.photos/id/23/200/300.jpg?hmac=NFze_vylqSEkX21kuRKSe8pp6Em-4ETfOE-oyLVCvJo"></a>
```

以及下面的 JavaScript:

```
const greet = () => {
  alert('Hello world!');
}
```

# 鼠标事件

我们可以通过使用属性来监听鼠标事件，并将它们的值设置为 JavaScript 代码。

例如，我们可以写:

```
<h1 onMouseover="alert('hello world.');">hello world</h1>
```

当鼠标悬停在 h1 元素上时显示一个警告框。

我们还可以添加`onMouseout`事件处理程序，当我们悬停在元素上时做一些事情，然后当我们将鼠标从元素上移开时反转它:

```
<h1 onMouseover="this.style.color='green'" onMouseout="this.style.color='black'">hello world</h1>
```

上面代码中的`this`是 h1 元素。

当我们将鼠标悬停在 h1 元素上时，我们将`color`样式设置为`'green'`。

然后当我们移开鼠标时，我们将`color`设置回黑色。

# 输入事件

同样，我们可以监听输入元素发出的事件。

例如，我们可以写:

```
<input type="text" size="30" onFocus="this.style.backgroundColor = 'yellow';" onBlur="this.style.backgroundColor = 'white';">
```

当我们关注输入元素时，我们添加了`onFocus`和`onBlur`处理程序来将`backgroundColor`样式设置为`'yellow'`。

然后，当我们将光标从输入处移开时，它会变回白色背景。

# 读取字段值

当我们提交表单时，我们可以读取输入字段中输入的值。

我们可以编写下面的 HTML:

```
<form onSubmit="checkAddress('email'); return false;">
  Email:
  <input type="text" id="email">
  <input type="submit" value="Submit">
</form>
```

和下面的 JavaScript 来检查电子邮件字段:

```
const checkAddress = (fieldId) => {
  if (document.getElementById(fieldId).value === "") {
    alert("Email is required.");
  }
  return false;
}
```

`checkAddress`函数获取 ID 为`email`的输入值。

如果它是空的，我们会在警报中看到`'Email is required'`消息。

我们需要`onSubmit`属性中的`return false`和`checkAddress`函数来防止默认提交行为。

我们可以通过编写以下代码来清理该函数:

```
const checkAddress = (fieldId) => {
  const {
    value
  } = document.getElementById(fieldId)
  if (value === "") {
    alert("Email is required.");
  }
  return false;
}
```

# 结论

我们可以通过监听按钮、鼠标和输入事件来监听来自各种输入设备的事件，并随心所欲地使用它们。