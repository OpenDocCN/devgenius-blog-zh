# 面向对象的 JavaScript —窗口的属性

> 原文：<https://blog.devgenius.io/object-oriented-javascript-properties-of-window-db0f80b2deb7?source=collection_archive---------7----------------------->

![](img/df1b4b3ce4a61580d75c6da3dc95be63.png)

由[罗布·温盖特](https://unsplash.com/@robwingate?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看`window`的内置属性。

# window.screen 属性

属性让我们获得关于浏览器屏幕的信息。

我们可以通过以下方式获得颜色深度:

```
window.screen.colorDepth;
```

我们可以得到屏幕的宽度:

```
screen.width;
screen.availWidth;
```

`width`是整个屏幕，`availableWidth`减去菜单和滚动条等的宽度。

我们可以用同样的方法得到高度:

```
screen.height;
screen.availHeight;
```

有些设备还具有`devicePixelRatio`属性:

```
window.devicePixelRatio
```

它告诉我们视网膜显示器中物理像素和设备像素之间的三重关系。

# window.open()/close()方法

`window.open`方法让我们打开一个新的浏览器窗口。

浏览器的各种政策和设置可能会阻止我们打开弹出窗口来抑制弹出广告。

但是我们应该能够打开一个新的窗口，如果它是由用户发起的。

如果我们试图在页面加载时打开一个窗口，它可能会被阻塞，因为用户没有打开这个窗口。

`window.open()`方法有几个参数。

第一个是加载到新窗口中的 URL。

新窗口的名称可以用作表单的`target`属性的值。

以逗号分隔的特性列表也是参数，其中包括`resizable`来指示弹出窗口是否可调整大小。

`width`是弹出窗口的宽度。

`status`是指示状态栏是否应该可见。

例如，我们可以写:

```
const win = window.open('http://www.example.com', 'example window',
'width=300,height=300,resizable=yes');
```

我们用 URL、标题和一个字符串调用`open`方法，该字符串包含窗口大小以及它是否可调整大小。

`win`有`close`方法让我们关闭窗口。

# window.moveTo()和 window.resizeTo()方法

`window.moveYo`方法让我们将浏览器窗口移动到左上角的给定屏幕坐标。

例如，我们可以写:

```
window.moveTo(100, 100)
```

移动窗户。

我们也可以调用`moveBy`方法，将窗口从当前位置上下移动给定的像素数。

例如，我们可以写:

```
window.moveBy(10, 10):
```

将窗口向右下移动 10 个像素。

我们也可以传入负数以向相反的方向移动。

`window.resizeTo`和`window.resizeBy`方法接受与移动方法相同的参数。

但是他们调整了窗口的大小，而不是移动它。

# window.alert()、window.prompt()和 window.confirm()方法

`alert`是一个方法，它接受一个字符串，并显示带有我们传入的文本的警告。

`confirm`让我们用想要显示的文本创建一个对话框。

用户可以单击“确定”或“取消”来关闭对话框。

该值将由`confirm`函数返回。

如果我们点击 OK，它将返回`true`，否则将返回`false`。

所以我们可以写:

```
const answer = confirm('Are you sure?');
```

然后我们可以在用户点击按钮后得到答案。

这对于确认用户操作很方便。

所以我们可以写:

```
if (confirm('Are you sure?')) {
  // ...
} else {
  // ...
}
```

`prompt`让我们显示一个带有一些问题文本的对话框，让用户输入一些内容。

输入的值将作为字符串返回。

如果它是空的，那么它返回一个空字符串。

如果用户单击 cancel、X 或按 Esc 键，它将返回`null`。

我们可以通过书写来使用它:

```
const answer = prompt('Are you sure?');
```

有输入的答案。

![](img/26e463b6ac525714de2fead1074ea9ae.png)

[Nathan Fertig](https://unsplash.com/@nathanfertig?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

`window`有很多方法我们可以用。

它们包括打开窗口、打开警告和提示等方法。