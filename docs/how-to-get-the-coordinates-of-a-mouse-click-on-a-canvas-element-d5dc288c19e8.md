# 如何获取鼠标点击画布元素的坐标？

> 原文：<https://blog.devgenius.io/how-to-get-the-coordinates-of-a-mouse-click-on-a-canvas-element-d5dc288c19e8?source=collection_archive---------2----------------------->

![](img/aa8fef7de29b6b207ac380ab5dca9183.png)

照片由 [Ricky Kharawala](https://unsplash.com/@sweetmangostudios?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有时，我们可能想获得鼠标在 HTML 画布元素上的坐标。

在本文中，我们将研究如何获取鼠标点击画布元素的坐标。

# 添加 mousedown 事件处理程序

我们可以从从`mousedown`事件处理程序获得的事件对象中获得鼠标点击的坐标。

例如，我们可以编写以下 HTML:

```
<canvas style="width: 200px; height: 100px">
</canvas>
```

然后我们可以编写下面的 JavaScript 代码:

```
const canvas = document.querySelector('canvas')
const ctx = canvas.getContext("2d");
ctx.moveTo(0, 0);
ctx.lineTo(200, 100);
ctx.stroke();const getCursorPosition = (canvas, event) => {
  const rect = canvas.getBoundingClientRect()
  const x = event.clientX - rect.left
  const y = event.clientY - rect.top
  console.log(x, y)
}canvas.addEventListener('mousedown', (e) => {
  getCursorPosition(canvas, e)
})
```

我们添加一个宽 200 像素，高 100 像素的画布元素。

然后我们用`document.querySelector`方法得到画布元素。

然后我们用`getContext`方法得到画布的上下文。

然后我们调用`moveTo`将光标移动到给定的 x，y 坐标。

我们称`lineTo`为给定的 x，y 坐标画一条线。

然后我们调用`stroke`来画线。

接下来，我们创建`getCursorPosition`函数来获取画布和事件参数。

我们调用`canvas.getBoundingClientRect`来获取`rect`对象，该对象具有`left`和`top`属性来获取左上角的 x 和 y 坐标。

然后我们从拥有我们点击的屏幕的鼠标坐标的`event.clientX`和`event.clientY`属性中减去这个值。

因此，`x`和`y`拥有我们在画布上点击的鼠标坐标。

最后，我们调用`canvas.addEventListener`来添加`mousedown`事件监听器。

在其中，我们调用`getCursorPosition`来获取我们在画布上单击的鼠标坐标并记录下来。

`x`和`y`以像素为单位。

此外，我们可以从`event`对象中获取`offsetX`和`offsetY`属性，以获得我们单击的画布的坐标。

例如，我们可以写:

```
const canvas = document.querySelector('canvas')
const ctx = canvas.getContext("2d");
ctx.moveTo(0, 0);
ctx.lineTo(200, 100);
ctx.stroke();const getCursorPosition = (canvas, event) => {
  const x = event.offsetX
  const y = event.offsetY
  console.log(x, y)
}canvas.addEventListener('mousedown', (e) => {
  getCursorPosition(canvas, e)
})
```

不用计算就能得到鼠标坐标。

# 结论

我们可以用来自`mousedown`事件对象的一些属性获得我们在画布上点击的位置的鼠标坐标。