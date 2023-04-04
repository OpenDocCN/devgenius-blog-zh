# Javascript IIFE 以及为什么使用它们？

> 原文：<https://blog.devgenius.io/javascript-iife-and-why-to-use-them-cbdff335f565?source=collection_archive---------9----------------------->

![](img/adc3892e1d53493061f3cdb896bde9f9.png)

> 在这个故事中，我将讲述 JavaScript**life**的一个有趣特性，以及在日常编码中使用它的原因。

**什么是生活？
life**代表***‘立即调用函数表达式’***这就很好地说明了问题。但简单来说，生命是通过声明立即运行的函数。也是 javascript 中公认的**自执行匿名函数**设计模式。每个生命都有两部分*宣告*和*召唤。*

**为什么要用 IIFE？**
life 是一个强大的小特性，可以帮助你管理 javascript 的全局上下文名称空间。

> ***避免全局变量命名空间冲突***

在现代 web 应用程序中，几乎每个网页都包含来自不同来源和脚本的 javascript。由于在一个网页中包含多个脚本，可能会出现全局变量命名空间冲突。

没有生命多个脚本声明相同的变量名。

在上面的例子中，有三个脚本声明了同名的全局变量 *glob_a* ，它覆盖了每个脚本中的值集。这可能会导致副作用。生活有助于避免这种冲突。

多个脚本声明了同一个变量名。

在上面的例子中，脚本中的所有 javascript 代码都包含在一个 IIFE 中。变量 *glob_a* 在每个生命中都有其独特的存在和价值。

> ***减少全局变量命名空间污染，提高性能。***

除了全局变量命名空间冲突，IIFE 还有助于减少过度使用全局变量造成的污染。

由于 IIFE，全局上下文没有变量

考虑上面的例子，注意，IIFE 不仅隔离了每个 *glob_a* 变量声明，还从全局上下文中删除了*glob_a* 变量。因此，它减少了全局变量的数量。IIFE 还提高了性能，因为变量 *glob_a* 现在位于局部上下文中，并且不会在全局上下文中查找 *glob_a* 。

[](https://medium.com/@tdillusionisme/comma-pipeline-nullish-coalescing-and-more-unique-javascript-operators-a4d8b85ca5c5) [## 逗号、管道、无效合并和更多独特的 Javascript 操作符

### 您应该掌握的独特的 Javascript 操作符

medium.com](https://medium.com/@tdillusionisme/comma-pipeline-nullish-coalescing-and-more-unique-javascript-operators-a4d8b85ca5c5)