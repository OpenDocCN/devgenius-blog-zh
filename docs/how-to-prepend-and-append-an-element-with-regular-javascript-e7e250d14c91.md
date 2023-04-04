# 如何用常规 JavaScript 预先计划和追加一个元素？

> 原文：<https://blog.devgenius.io/how-to-prepend-and-append-an-element-with-regular-javascript-e7e250d14c91?source=collection_archive---------1----------------------->

![](img/fa9058d82b8072130df8ba4a231b3708.png)

照片由[克里斯蒂安·M](https://unsplash.com/@omsengo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有时，我们希望在网页上的父元素中预先计划或附加一个子元素。

在本文中，我们将研究如何用常规 JavaScript 预先计划和附加一个元素。

# 前置一个元素

我们可以通过使用`insertBefore`方法预先计划一个元素。

例如，如果我们有下面的 HTML:

```
<div id='parent'>
  <p>
    hello world
  </p>
</div>
```

然后，我们可以在 div 中的`p`元素之前添加一个元素，方法是:

```
const parent = document.getElementById("parent");
const child = document.createElement("div");
child.innerHTML = 'Are we there yet?';
parent.insertBefore(child, parent.firstChild);
```

我们用`document.getElementById`得到 div。

然后我们用`document.createElement`创建一个 div。

然后我们通过设置`innerHTML`属性向`child` div 添加一些内容。

最后，我们用我们想要插入的`child`元素和`parent.firstChild`调用`parent.insertBefore`以在`parent`的第一个子节点之前添加`child`。

# 追加元素

我们可以使用`appendChild`方法将元素添加到容器元素中。

例如，如果我们有下面的 HTML:

```
<div id='parent'>
  <p>
    hello world
  </p>
</div>
```

然后我们可以在`p`元素后添加一个子元素，方法是:

```
const parent = document.getElementById("parent");
const child = document.createElement("div");
child.innerHTML = 'Are we there yet?';
parent.appendChild(child);
```

前 3 行与前一个示例相同。

唯一的区别是我们用`child`调用`parent.appendChild`来在`p`元素后添加`child`。

# 结论

我们可以使用`insertBefore`方法预先计划一个元素作为第一个子元素。

我们可以使用`appendChild`方法添加一个子元素作为父元素的最后一个子元素。