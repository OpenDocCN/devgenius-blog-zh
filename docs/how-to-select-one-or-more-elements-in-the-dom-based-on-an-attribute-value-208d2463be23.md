# 如何根据属性值在 DOM 中选择一个或多个元素？

> 原文：<https://blog.devgenius.io/how-to-select-one-or-more-elements-in-the-dom-based-on-an-attribute-value-208d2463be23?source=collection_archive---------4----------------------->

![](img/7b4498b047d0cbe8c6cd7e311bc4ce93.png)

micha Parzuchowski 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

有时，我们希望在 DOM 中选择一个或多个具有给定属性和值的元素。

在本文中，我们将研究如何根据属性值在 DOM 中选择一个或多个元素。

# 基于精确的属性值在 DOM 中选择一个或多个元素

为了根据属性值选择 DOM 中的一个或多个元素，我们可以向`querySelector`中传递一个选择器来选择一个具有给定属性值的元素。

要选择具有给定属性值的所有元素，我们可以使用`querySelectorAll`方法。

选择器的一般格式是:

```
tagName[attribute="value"]
```

例如，如果我们想选择一个带有给定的`src`属性值的图像元素，给出如下 HTML:

```
<img src='https://imaging.nikon.com/lineup/dslr/df/img/sample/img_01.jpg'>
```

我们可以写:

```
const img = document.querySelector("img[src='https://imaging.nikon.com/lineup/dslr/df/img/sample/img_01.jpg']");
console.log(img)
```

我们称`querySelector`为标签名`img`，

而`src`属性值为`https://imaging.nikon.com/lineup/dslr/df/img/sample/img_01.jpg`。

这将选择具有给定标记名和属性值的第一个元素。

如果我们想要选择具有给定标记名和属性值的所有元素，我们可以用`querySelectorAll`替换`querySelector`。

# 在包含给定属性值的 DOM 中选择一个或多个元素

我们可以在`=`符号前添加一个`~`来获得一个或多个包含给定属性值的元素。

例如，对于同一个 HTML，我们可以编写:

```
const img = document.querySelector("img[src~='https://imaging.nikon.com/lineup/dslr/df/img/sample/img_01.jpg']");
console.log(img)
```

我们得到了具有包含`https://imaging.nikon.com/lineup/dslr/df/img/sample/img_01.jpg`的`src`属性的`img`元素。

我们可以用`querySelectorAll`替换`querySelector`来获得所有包含给定 URL 的`src`属性的`img` 元素。

# 结论

我们可以用一些简单的 CSS 选择器和`querySelector`或`querySelectorAll`方法选择具有给定属性值的元素。