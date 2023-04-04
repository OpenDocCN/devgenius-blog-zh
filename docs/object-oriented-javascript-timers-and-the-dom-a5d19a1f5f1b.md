# 面向对象的 JavaScript——计时器和 DOM

> 原文：<https://blog.devgenius.io/object-oriented-javascript-timers-and-the-dom-a5d19a1f5f1b?source=collection_archive---------5----------------------->

![](img/581ba843c322b9fe2fa34beaa5f66ff8.png)

照片由[维里·伊万诺娃](https://unsplash.com/@veri_ivanova?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看`window`和 DOM 的内置属性。

# window.setTimeout()和 window.setInterval()方法

我们可以使用`window.setTimeout`和`window.setInterval`方法来调度一些 JavaScript 代码的执行。

`setTimeout`让我们在延迟后运行一些代码。

例如，我们可以写:

```
function foo() {
  alert('foo');
}
setTimeout(foo, 2000);
```

它返回计时器的 ID。

我们可以用这个 ID 给`clearTimeout`打电话取消计时器。

例如，我们可以写:

```
function foo() {
  alert('foo');
}
const id = setTimeout(foo, 2000);
clearTimeout(id);
```

那么警报将永远不会显示，因为 2 秒钟前的`clearTimeout`已经结束。

我们可以使用`setInterval`方法来调度`foo`每 2 秒运行一次。

例如，我们可以写:

```
function foo() {
  console.log('foo');
}
const id = setInterval(foo, 2000);
```

我们可以通过编写以下代码调用`clearInterval`来取消计时器:

```
clearInterval(id);
```

将一个函数调度在几毫秒内并不能保证它就在那个时间执行。

大多数浏览器没有毫秒级的解析时间。

此外，浏览器维护了一个我们期望他们做什么的队列。

100 毫秒意味着在 100 毫秒后加入队列

但是队列可能会被一些慢的东西延迟。

大多数最新的浏览器都有比超时功能更好的`requestAnimationFrame`功能，因为我们要求浏览器只要有资源就运行。

计划不是以毫秒为单位定义的。

例如，我们可以写:

```
function animateDate() {
  requestAnimationFrame(() => {
    console.log(new Date());
    animateDate();
  });
}
animateDate();
```

每当浏览器可以运行代码时记录最新日期。

# window.document 属性

`window.document`属性是一个 BOM 对象，它引用当前加载的文档或页面。

# 数字正射影像图

DOM 将 XML 或 HTML 文档表示为节点树。

我们可以使用 DOM 方法和属性来访问页面上的任何元素。

此外，我们可以使用它们来修改或删除元素。

这是一个独立于语言的 API，可以用任何语言实现。

我们可以在浏览器的 Elements 选项卡中查看 DOM 树。

例如，我们应该看到一个`p`元素是由`HTMLParagraphElement`构造函数创建的。

head 标签是由`HTMLHeadElement`构造函数创建的。

# 核心 DOM 和 HTML DOM

核心 DOM 构成了 DOM 规范的最基本部分。

HTML DOM 规范建立在核心 DOM 之上。

以下构造函数位于核心 DOM 中:

*   `Node` —树上的任何节点
*   `Document` —文档对象
*   `Element` —树上的标签
*   `CharacterData` —处理文本的构造器
*   `Text` —标签中的文本节点
*   `Comment` —文档中的注释
*   `Attr` —标签的属性
*   `NodeList` —节点列表，这是一个具有`length`属性的类似数组的对象
*   `NamedNodeMap` —与`NodeList`相同，但是可以通过名称而不是数字索引来访问节点。

HTML DOM 有以下构造函数:

*   `HTMLDocument`—`document`对象的 HTML 特定版本
*   `HTMLElement` —所有 HTML 元素的父构造函数
*   `HTMLBodyElement` —表示主体标签的元素
*   `HTMLLinkElement`—`a`元素
*   `HTMLCollection` —一个特定于 HTML 的`NamedNodeMap`

我们可以使用它们来访问 DOM 节点，并修改、创建和删除节点。

![](img/1794127ac24d3775a3f457d116fe89b8.png)

照片由[Siri wan arunsiriwatana](https://unsplash.com/@muay0051?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

HTML 文档由 DOM 表示。

我们可以获取并操纵 DOM 树中的节点来做我们想做的事情。

`window`有定时器方法让我们调度代码运行。