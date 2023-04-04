# 如何在 JavaScript 控制台日志输出中添加颜色？

> 原文：<https://blog.devgenius.io/how-to-add-colors-in-javascript-console-log-output-e41efeffa8dd?source=collection_archive---------0----------------------->

![](img/9a6f394793472b1b70e86a393dc34feb.png)

照片由[杰瑞米·托马斯](https://unsplash.com/@jeremythomasphoto?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

为了让控制台长输出更容易看到，我们可以给 JavaScript 的控制台日志添加颜色。

在本文中，我们将了解如何在浏览器开发控制台中为控制台日志输出添加颜色。

# 向控制台日志输出添加 CSS 样式

我们可以用`%c`标签向控制台日志输出添加 CSS 样式。

例如，我们可以写:

```
console.log('%c hello world ', 'background: #222; color: #bada55');
```

我们添加了`%c`标签，这样我们可以将第二个参数中的 CSS 样式应用到`hello world`文本中。

我们设置背景属性来设置背景颜色。

我们设置颜色属性来改变文本颜色。

# 控制台. error 方法

`console.error`方法让我们用红色背景记录事情。

这让我们能够以更明显的方式显示数据。

例如，我们可以写:

```
console.error("hello world");
```

使用方法。

# console.warn 方法

`console.warn`方法让我们在黄色背景上记录东西。

为了使用它，我们写:

```
console.warn("hello world");
```

# Bash 彩旗

另外，`console.log`让我们添加 Bash 颜色标志来设置文本的颜色。

例如，我们可以写:

```
console.log('\x1B[31mhello\x1B[34m world');
```

`\x1B[31`意为红色。

而`\x1B[34m`是蓝色的。

此外，我们可以通过书写来设置文本的背景颜色:

```
console.log('\x1b[43mhighlighted');
```

`\x1b[43m`将背景颜色设置为黄色。

# 结论

我们可以用各种技巧将彩色文本记录到浏览器开发控制台。

一些`console`方法让我们以各种方式改变颜色。