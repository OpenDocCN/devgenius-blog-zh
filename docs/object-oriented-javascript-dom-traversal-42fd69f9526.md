# 面向对象的 JavaScript — DOM 遍历

> 原文：<https://blog.devgenius.io/object-oriented-javascript-dom-traversal-42fd69f9526?source=collection_archive---------7----------------------->

![](img/040f9351b4d3550c073b738c9156fcd1.png)

照片由[卢卡·布拉沃](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究 DOM。

# 访问 DOM 节点

我们可以在浏览器中访问带有内置对象的 DOM 节点。

`document`节点让我们可以访问当前文档。

我们可以用`console.dir`的方法来看它的性质:

```
console.dir(document);
```

我们可以使用`nodeType`属性来获取节点的类型。

`ndoeName`有节点的名称。

`nodeValue`有值，该值是以文本为例的文本节点。

的:

```
document.nodeType;
```

返回 9。

有 12 种节点类型，用整数表示。

最常见的是 1 表示元素，2 表示属性，3 表示文本。

```
document.nodeName
```

返回`'#document'`。

而`document.nodeValue`就是`null`。

# documentElement

属性为我们提供了 HTML 元素。

`nodeType`是 1，因为它是一个元素。

我们可以看到:

```
document.documentElement.nodeType
```

我们可以用以下方式得到`nodeName`:

```
document.documentElement.nodeName
```

并使用以下内容获取标记名:

```
document.documentElement.tagName
```

他们都返回`'HTML'`。

# 子节点

我们可以从`document`对象中获得子节点。

同样，我们可以用`hasChildNodes`方法检查是否有子节点。

例如，我们可以写:

```
document.documentElement.hasChildNodes();
```

我们得到了`true`。

`childNodes`属性具有`length`属性来获取一个 DOM 节点的子节点数。

例如，我们可以写:

```
document.documentElement.childNodes.length;
```

我们可以通过它的索引得到子节点:

```
document.documentElement.childNodes[0];
document.documentElement.childNodes[1];
```

等等。

要获得父节点，我们可以使用`parentNode`属性:

```
document.documentElement.childNodes[1].parentNode;
```

它们都返回一些元素。

在最后一个示例中，返回 HTML 元素。

例如，如果我们有:

```
<body>
  <p class="opener">foo</p>
  <p><em>bar</em> </p>
  <p id="closer">baz</p>
  <!-- the end -->
</body>
```

然后`console.log(document.body.childNodes.length)`记录 10，因为我们有 HTML 元素、属性节点、文本节点和注释节点。

# 属性

我们可以用`hasAttributes`方法检查一个元素是否有属性。

例如，我们可以写:

```
console.log(document.body.childNodes[1].hasAttributes())
```

然后我们得到`true`。

如果是`true`,那么这个节点就有属性。

我们可以用`attributes`得到属性。

例如，我们可以写:

```
console.log(document.body.childNodes[1].attributes[0].nodeName)
```

然后我们得到`'class'`。

`nodeName`获取属性的名称。

我们还可以使用`nodeValue`属性来获取属性值。

所以我们可以写:

```
console.log(document.body.childNodes[1].attributes['class'].nodeValue)
```

我们也可以用`getAttribute`方法得到属性:

```
console.log(document.body.childNodes[1].getAttribute('class'))
```

# 访问标签内的内容

我们可以在标签中获取内容。

为此，我们可以使用`textContent`属性来获取标签内的文本内容。

所以我们可以写:

```
console.log(document.body.childNodes[1].textContent)
```

然后我们得到`'foo'`。

此外，我们可以获取`innerHTML`属性来获取 HTML 元素节点中的 HTML 内容:

```
console.log(document.body.childNodes[3].innerHTML)
```

我们得到了:

```
<em>bar</em>
```

和其他元素一样，我们可以得到`length`、`nodeName`和`nodeValue`。

例如，我们可以写:

```
console.log(document.body.childNodes[1].childNodes.length)
```

获取子节点的长度，并且:

```
console.log(document.body.childNodes[0].nodeName)
```

哪些日志`‘#text’`和:

```
console.log(document.body.childNodes[1].childNodes[0].nodeValue)
```

我们得到了`'foo'`。

![](img/96b6788230384d5f36c6a6c955efb3b5.png)

尼克·佩雷斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以使用各种属性来获取 DOM 节点和属性。