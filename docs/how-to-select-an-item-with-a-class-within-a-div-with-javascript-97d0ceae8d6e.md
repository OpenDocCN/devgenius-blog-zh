# 如何用 JavaScript 在 Div 内选择带有类的项目？

> 原文：<https://blog.devgenius.io/how-to-select-an-item-with-a-class-within-a-div-with-javascript-97d0ceae8d6e?source=collection_archive---------0----------------------->

![](img/848d966f82f8c7a89bd41104ac208996.png)

[金伯利农民](https://unsplash.com/@kimberlyfarmer?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

有时，我们希望用 JavaScript 在 div 中选择一个带有类的项目。

在本文中，我们将看看如何用 JavaScript 在 div 中选择一个带有类的项目。

# 使用 document.querySelector 选择器

我们可以使用`document.querySelector` on 来选择一个 div，然后用给定的类选择其中的一个元素。

例如，如果我们有下面的 HTML:

```
<div id="mydiv">
  <div class="myclass"></div>
</div>
```

然后，我们可以通过编写以下代码来选择带有类`myclass`的 div:

```
const innerDiv = document
  .querySelector('#mydiv')
  .querySelector('.myclass')
console.log(innerDiv)
```

我们只是在 ID 为`mydiv`的元素上调用`querySelector`来选择 div 中的项目。

因此，`innerDiv`就是带有`myclass`类的 div。

# 使用 document.getElementById

我们可以用`getElementById`代替第一个`querySelector`调用。

例如，如果我们有下面的 HTML:

```
<div id="mydiv">
  <div class="myclass"></div>
</div>
```

然后，我们可以通过编写以下代码来选择带有类`myclass`的 div:

```
const innerDiv = document
  .getElementById('mydiv')
  .querySelector('.myclass')
console.log(innerDiv)
```

然后我们看到和之前一样的结果。

# 使用 document . getelementsbyclassname

此外，我们可以使用`getElementsByClassName`方法获取元素中给定类的所有元素。

例如，如果我们有下面的 HTML:

```
<div id="mydiv">
  <div class="myclass"></div>
</div>
```

然后，我们可以通过编写以下代码来选择带有类`myclass`的 div:

```
const [innerDiv] = document
  .getElementById('mydiv')
  .getElementsByClassName('myclass')
console.log(innerDiv)
```

我们用类`myclass`从`getElementsByClassName`返回的元素中重构第一个 div。

我们得到了和以前一样的结果。

# 结论

我们可以通过浏览器内置的元素选择方法来获取元素中的元素。