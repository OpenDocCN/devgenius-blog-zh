# 如何用 JavaScript 隐藏或显示元素？

> 原文：<https://blog.devgenius.io/how-to-hide-or-show-elements-with-javascript-c608f6159aad?source=collection_archive---------2----------------------->

![](img/e5e7c76bce2cd4d469505b6ba40a1e5b.png)

照片由[Dawid za wia](https://unsplash.com/@davealmine?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在许多情况下，我们可能希望创建可以通过切换来显示和隐藏的组件。

在本文中，我们将看看如何用 JavaScript 隐藏或显示元素。

# 用 JavaScript 隐藏或显示元素

我们可以用 JavaScript 通过设置元素的`style.display`属性来显示或隐藏元素。

我们可以通过设置为`'none'`来隐藏它。

我们可以通过设置为`'block'`来显示它。

例如，我们可以编写以下 HTML:

```
<button>
  toggle
</button>
<p>
  hello world
</p>
```

然后我们可以添加以下 JavaScript 代码:

```
const button = document.querySelector('button')
const p = document.querySelector('p')button.addEventListener('click', () => {
  if (p.style.display === 'none') {
    p.style.display = 'block'
  } else {
    p.style.display = 'none'
  }
})
```

我们在 HTML 中添加一个按钮和 p 元素。

然后我们用`document.querySelector`选择元素。

然后我们用`button.addEventListener`方法添加一个点击事件监听器。

我们检查`p.style.display`是否是`'none'`。

如果是`'none'`，那么我们将`p.style.display`设置为`'block'`。

否则，我们将其设置为`'none'`。

`'none'`使元素消失。没有为该元素分配空间。

并且`'block'`使其显示为块元素。

现在，当我们点击按钮时，我们看到 p 元素被打开和关闭。

或者，我们可以设置`visibility`属性。

`visibility`和`display`的区别在于`visibility`为元素分配空间，不管它是否显示。

`display`就不是这样了。

所以我们也可以写:

```
const button = document.querySelector('button')
const p = document.querySelector('p')button.addEventListener('click', () => {
  if (p.style.visibility === 'hidden') {
    p.style.visibility = 'visible'
  } else {
    p.style.visibility = 'hidden'
  }
})
```

在我们的例子中我们得到了相同的结果。

# 结论

我们可以将`display`属性设置为`'block'`或者将`visibility`设置为`'visible'`来显示一个元素。

我们可以将`display`属性设置为`'none'`或者`visibility`到`'hidden'`来隐藏一个元素。