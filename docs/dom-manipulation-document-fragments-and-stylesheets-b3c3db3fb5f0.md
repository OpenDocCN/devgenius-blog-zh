# DOM 操作—文档片段和样式表

> 原文：<https://blog.devgenius.io/dom-manipulation-document-fragments-and-stylesheets-b3c3db3fb5f0?source=collection_archive---------10----------------------->

![](img/34142a7b84dd2badb68aeab070125287.png)

伊夫琳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将研究如何操作文档片段并从样式表中获取信息。

# 文档碎片

`DocumentFragment`是一种提供轻量级文档 DOM 的节点，它位于动态 DOM 树的外部。

这是一个文档模板，其行为类似于真实的模板，但却存在于内存中。

它的子节点可以很容易地在内存中操作，并附加到动态 DOM 中。

# 创建文档片段

`createDocumentFragment`方法让我们创建文档片段。

例如，我们可以写:

```
const frag = document.createDocumentFragment();for (const s of ['foo', 'bar', 'baz']) {
  const p = document.createElement("p");
  p.textContent = s;
  frag.appendChild(p);
}
```

我们用`createDocumentFragment`创建了一个文档片段。

然后，我们遍历数组字符串来创建 p 元素，并将它们作为片段的子元素追加。

创建文档片段的好处是我们可以将它存储在内存中。

它可以包含除`body` 或`html`之外的任何类型的节点。

当我们追加一个片段时，片段本身并没有被添加到 DOM 中。

只添加其中的节点。

当一个文档片段被附加到 DOM 时，它从文档转移到它被附加的地方。

它会从记忆中消失。

另一方面，元素总是在内存中。

# 向 DOM 添加 DocumentFragment

我们可以将文档片段传递给`appendchild`并将`insertBefore`传递给另一个元素。

例如，我们可以将上一个示例中的内容附加到正文中，方法是:

```
const frag = document.createDocumentFragment();for (const s of ['foo', 'bar', 'baz']) {
  const p = document.createElement("p");
  p.textContent = s;
  frag.appendChild(p);
}document.body.appendChild(frag);
```

现在，我们将创建的 p 元素添加到主体中。

# 在文档片段上使用 innerHTML

我们可以使用`innerHTML`用一个字符串来设置片段的内容。

例如，我们可以通过编写以下内容来创建文档片段:

```
const frag = document.createDocumentFragment();const div = document.createElement('div');
frag.appendChild(div);
frag.querySelector('div').innerHTML = '<p>foo</p><p>bar</p>';document.body.appendChild(frag);
```

我们用`createFragment`创建一个片段。然后我们创建一个 div 来保存内容。

然后我们将 div 的内容设置为我们自己的 HTML。

最后，用`appendChild`将片段内容追加到正文中。

# 将包含节点的片段留在内存中

如果我们想保留片段的内容，我们可以克隆它。

例如，如果我们有下面的片段:

```
const frag = document.createDocumentFragment();for (const s of ['foo', 'bar', 'baz']) {
  const p = document.createElement("p");
  p.textContent = s;
  frag.appendChild(p);
}
```

我们可以这样写来克隆它:

```
document.body.appendChild(frag.cloneNode(true));
```

然后，我们保留文档片段内容，同时将它附加到主体。

# CSS 样式表

我们可以使用 link 标签给页面添加样式。

此外，我们可以向文档添加样式标签或样式属性。

添加到 HTML 文档中的每个样式表都由`CSSStylesheer`对象表示。

如果我们有一个样式元素:

```
<style>
  body {
    background-color: green;
  }
</style>
```

我们可以通过编写以下代码用 JavaScript 检索样式表:

```
console.log(document.querySelector('style').sheet);
```

我们得到一个带有样式表的对象。

# 访问所有样式表

我们可以使用`styleSheets`属性来获取所有的样式表。

例如，如果我们有以下 CSS:

```
<!doctype html>
<html lang="en"> <head>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
    <style>
      body {
        background-color: #fff;
      }
    </style>
  </head>
  <body>
  </body>
</html>
```

然后我们可以得到带有以下内容的样式表:

```
console.log(document.styleSheets);
```

我们也可以使用链接元素的`sheet`属性，如下所示:

```
console.log(document.querySelector('link').sheet);
```

![](img/90d7c7a8636f0198fcd1f8356e9549b1.png)

[zo Reeve](https://unsplash.com/@zoeeee_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# CSS 样式表属性和方法

一个`CSSStyleSheet`有属性和方法。

他们有以下内容:

*   `disabled` —指示样式表是否被禁用
*   `href`—`href`属性的值
*   `media` —风格信息的目的媒体
*   `ownerNode` —将样式表与文档相关联的节点
*   `parentStylesheet` —父样式表
*   `title`—`title`属性的值
*   `type`—`type`属性的值
*   `cssRules` —类似数组的对象，用于保存组成样式表的规则
*   `ownerRule` —保存导入样式表的导入规则
*   `deleteRule` —从样式表对象中删除规则的方法
*   `insertRule` —将规则插入样式表的方法

# 结论

我们可以在浏览器 JavaScript 代码中获取样式表数据。

此外，我们可以从 DOM 中的样式表获取数据。