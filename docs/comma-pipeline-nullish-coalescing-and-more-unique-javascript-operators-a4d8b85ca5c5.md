# 逗号、管道、无效合并和更多独特的 Javascript 操作符

> 原文：<https://blog.devgenius.io/comma-pipeline-nullish-coalescing-and-more-unique-javascript-operators-a4d8b85ca5c5?source=collection_archive---------8----------------------->

**你应该得到的独特的 Javascript 操作符**

![](img/b18f91a2bcf72380464f36fee658551f.png)

> 运算符是任何编程语言的基本块之一。它们帮助我们执行各种操作以获得结果。在这个故事中，我将介绍一些您可以在日常编码中使用的独特的 javascript 操作符。

**逗号运算符(，)** 逗号运算符用于执行行内多个表达式。关于逗号操作符的一个有趣的事情是，它返回最后一个表达式的结果。

**零化合并算子(？？)** 当左操作数为 *null* 或 *undefined* 时，Nullish 合并运算符返回右操作数。但是，与逻辑 OR (||)运算符不同，它不会对左操作数执行类型强制(比较的类型转换),如果左操作数不为空或未定义，也不会对右操作数求值。Nullish 合并不能与其他运算符链接。

**在运算符中(中的*)*** 如果对象或其原型链中存在属性，则返回 *true* 。

如果你从对象中删除一个属性，那么中的*将返回*假*，但是如果你使属性*未定义，那么*中将返回*真*。*

**可选的链接运算符(？)** 该操作符检查一个属性是否为 *null* 或 *undefined* ，如果找到一个，则停止链的执行并返回 *undefined* 。它有助于在一个非常常见的场景中添加安全检查，在这个场景中，您希望执行一系列属性和函数，并且您不确定是否所有的属性和函数都已定义。

**取幂运算符(**)** 这是一个执行取幂运算的简单运算符。

**管道操作符(| > )** 该操作符处于实验阶段，尚不可用。但是，我还是想分享一下。
管道操作符将被用来把一个表达式传递给一个函数。这个操作符可以在链式函数调用中使用，以提高代码的可读性。

javascript 中有更多有趣的操作符，您可以尝试，如果有任何问题，请在回复部分告诉我。

[](https://blog.usejournal.com/javascript-lexical-grammar-strings-getter-and-setter-443f53c79743) [## Javascript 词汇语法、字符串、Getter 和 Setter

### 您可能会错过的 Javascript 特性

blog.usejournal.com](https://blog.usejournal.com/javascript-lexical-grammar-strings-getter-and-setter-443f53c79743)