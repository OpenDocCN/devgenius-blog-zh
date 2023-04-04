# 如何用 Vanilla JavaScript 求一个元素的宽度？

> 原文：<https://blog.devgenius.io/how-to-find-the-width-of-an-element-using-vanilla-javascript-10a154c39dc0?source=collection_archive---------5----------------------->

![](img/861e3920a389b2b8623afaecba09e6d8.png)

[Uta Scholl](https://unsplash.com/@uta_scholl?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时候，我们想用普通的 JavaScript 找到一个元素的宽度。

在本文中，我们将看看如何用普通的 JavaScript 找到元素的宽度。

# HTMLElement.offsetWidth

我们可以用来寻找元素宽度的一个属性是`offsetWidth`属性。

它是一个只读属性，以整数形式返回元素的布局宽度。

它以元素的 CSS 宽度的像素来度量，包括边框、填充和垂直滚动条(如果呈现的话)。

而且不包括任何伪元素的宽度。

如果元素是隐藏的，则返回 0。

例如，我们可以编写以下 HTML:

```
<p>
  hello world
</p>
```

那么我们可以通过写下得到`offsetWidth`:

```
const p = document.querySelector('p');
console.log(p.offsetWidth)
```

# HTMLElement.clientWidth

我们可以用来获取元素宽度的另一个属性是`clientWidth`属性。

对于内联元素和没有 CSS 的元素是零。

否则，它是以像素为单位的元素的内部宽度。

它包括填充，但不包括边框、边距和垂直滚动条(如果有)。

例如，我们可以编写以下 HTML:

```
<p>
  hello world
</p>
```

那么我们可以通过写得到`offsetWidth`:

```
const p = document.querySelector('p');
console.log(p.clientWidth)
```

得到`clientWidth`的财产。

# 使用 getComputedStyle 函数

`getComputedStyle`全局函数返回一个对象，该对象包含应用了活动样式表和基本计算的元素的所有 CSS 属性值。

我们可以从返回的对象中访问单独的 CSS 属性。

在样式表之后，`width`属性将计算元素的宽度，用 JavaScript 改变宽度，等等。都适用于它。

例如，我们可以写:

```
const p = document.querySelector('p');
console.log(getComputedStyle(p).width)
```

然后我们得到 p 元素的宽度，单位是字符串。

# 结论

我们可以使用各种属性和方法通过普通的 JavaScript 获得元素的宽度。