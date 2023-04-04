# 顶级的接近就绪的 JavaScript 特性

> 原文：<https://blog.devgenius.io/top-starred-near-ready-javascript-features-40c8ed3630bc?source=collection_archive---------7----------------------->

![](img/27fc1c8512b6bdec3d37607c395fcbdf.png)

JS 是不断发展的，在这篇文章中，我想谈谈已经完成的和即将问世的内容。社区已经欣赏了这些创新，并期待着它们。

JS 有几个级别的新特性批准。在本文中，我将讨论第 2 级和第 3 级的特性。

# 第二阶段提案

第二层的提议使用正式的规范语言精确地描述语法和语义。

让我们来看看第二阶段最有趣的功能:

## **1。**装修工

装饰器`@decorator`是在定义期间在类元素或其他 JavaScript 语法形式上调用的*函数*，潜在地*包装*或*用装饰器返回的新值替换*它们。

修饰类字段被视为包装 getter/setter 对以访问该存储。修饰存储对于观察/跟踪很有用，这一直是最初的遗留/实验性修饰器与类字段的[[Define]]语义相结合的一个痛点。这些语义基于 Michel Weststrate 的[“诱捕装饰者”提议](https://github.com/tc39/proposal-decorators/issues/299)。

装饰者也可以用*元数据*来注释一个类元素。这些是简单的、不受限制的对象属性，它们是从所有添加它们的装饰者那里收集来的，并作为一组嵌套对象在`[Symbol.metadata]`属性中可用。

例:
**@已记录。当一个方法开始和结束时，`@logged`装饰器记录一个控制台消息**

`@logged`可以作为装饰器在 JavaScript 中实现。

阅读更多关于提案的信息:[https://github.com/tc39/proposal-decorators](https://github.com/tc39/proposal-decorators)

## 2.记录和元组

这个提议为 JavaScript 引入了两个新的深度不可变的数据结构:

*   `Record`，一个深不可变的类物体结构`#{ x: 1, y: 2 }`
*   `Tuple`，一个深度不可变的类数组结构`#[1, 2, 3, 4]`

记录和元组只能包含基元和其他记录和元组。您可以将记录和元组视为“复合原语”。由于完全基于原语，而不是对象，记录和元组是完全不可变的。

示例:

## `Record`

## `Tuple`

阅读更多关于提案的信息:[https://github.com/tc39/proposal-record-tuple](https://github.com/tc39/proposal-record-tuple)

## 3.暂时的

`Date`一直是 ECMAScript 长期以来的痛点。这就提出了`Temporal`，一个充当顶级名称空间的全局`Object`(就像`Math`)，它为 ECMAScript 语言带来了一个现代的日期/时间 API。关于动机的详细分类，请参见:[修复 JavaScript 日期](https://maggiepint.com/2017/04/09/fixing-javascript-date-getting-started/)

Temporal 通过以下方式解决这些问题:

*   为日期和时间计算提供易于使用的 API
*   对所有时区的一流支持，包括 DST 安全算法
*   只处理不可变的对象
*   解析严格指定的字符串格式
*   支持非公历日历

示例:

1.  `Temporal.now`对象有几个给出当前时间和日期信息的方法。
2.  A `Temporal.Instant`代表一个固定的时间点(称为“精确时间”)，不考虑日历或位置。
3.  `Temporal.ZonedDateTime`是一种时区感知、日历感知的日期/时间类型，表示从地球上特定地区的角度来看，在特定的确切时间已经发生(或将发生)的真实事件。
4.  `Temporal.Date`对象表示与特定时间或时区无关的日历日期。
5.  `Temporal.Time`对象表示与特定日期或时区无关的挂钟时间。
6.  `Temporal.DateTime`代表日历日期和挂钟时间，不包含时区信息
7.  一个`Temporal.Duration`表示时间的长度。这用于日期/时间运算和测量`Temporal`对象之间的差异。
8.  `Temporal.TimeZone`代表 IANA 时区、特定的 UTC 时差或 UTC 本身。
9.  一个`Temporal.Calendar`代表一个日历系统。大多数代码将使用 ISO 8601 日历，但其他日历系统也是可用的。

阅读更多关于提案的信息:[https://github.com/tc39/proposal-temporal](https://github.com/tc39/proposal-temporal)

## 4.领域

领域是一个独特的全局环境，有自己的全局对象，包含自己的内部函数和内置函数。

Realms 提案提供了一种新的机制，在新的全局对象和 JavaScript 内置集的上下文中执行 JavaScript 代码。

API 能够控制一个领域内不同程序的执行，为虚拟化提供了一个合适的机制。这在今天的 Web 平台上是不可能的，而提议的 API 旨在为所有 JS 环境提供一个无缝的解决方案。

领域可以很好地应用于各种示例:

*   基于 Web 的 ide 或任何类型的使用同源评估策略的第三方代码执行。
*   DOM 虚拟化(例如:Google AMP)
*   测试框架和报告器(浏览器内测试，也可以在节点中使用`vm`)。
*   测试/模拟(例如:jsdom)
*   大多数网络插件机制(如电子表格功能)。
*   沙盒(例如:绿洲项目)
*   服务器端渲染(避免冲突和数据泄漏)
*   浏览器内代码编辑器
*   浏览器内翻译

该提案的主要目标是提供一种适当的机制来控制程序的执行，提供一个新的全局对象、一组新的内部函数、对孵化器领域的全局对象的无默认访问、一个单独的模块图以及与孵化器领域的同步通信。

例子

阅读更多关于提案的信息:[https://github.com/tc39/proposal-realms](https://github.com/tc39/proposal-realms)

# 第三阶段提案

第三层的建议表明，进一步的改进将需要来自实现和用户的反馈。

让我们来看看最令人期待的第 3 阶段功能:

## 1.类字段声明

这个提议的目的是提供在类体内声明字段的能力。

示例:

在上面的例子中，您可以看到一个用语法`x = 0`声明的字段。你也可以声明一个没有初始化器的字段为`x`。通过预先声明字段，类定义变得更加自文档化；实例经历更少的状态转换，因为声明的字段总是存在的。

要使字段私有，只需给它们一个以`#`开头的名称。

阅读有关提案的更多信息:[https://github.com/tc39/proposal-class-fields](https://github.com/tc39/proposal-class-fields)

## 2.顶级`await`

顶级`await`使模块能够充当大型异步函数:有了顶级`await`，ECMAScript 模块(ESM)可以`await`资源，导致其他模块在开始评估它们的身体之前等待。

顶层`await`让我们依靠模块系统本身来处理所有这些承诺，并确保事情得到很好的协调。上面的例子可以简单地写成和使用如下:

在`awaiting.mjs`中的`await`的承诺得到解决之前，`usage.mjs`中的任何语句都不会执行，因此通过设计避免了竞争情况。这是一个扩展，如果`awaiting.mjs`没有使用顶级`await`，那么`usage.mjs`中的任何语句都不会执行，直到`awaiting.mjs`被加载并且它的所有语句都被执行。

阅读更多关于提案的信息:[https://github.com/tc39/proposal-top-level-await](https://github.com/tc39/proposal-top-level-await)

还有更多第二阶段和第三阶段的提议，在这里阅读它们—[https://github.com/tc39/proposals](https://github.com/tc39/proposals)

感谢阅读！

P.S .如果你想在早期阶段了解更多关于最有趣的提议，请阅读此[https://medium . com/dev-genius/top-starred-JavaScript-features-at-early-phase-982 de 948 F2 AE](https://medium.com/dev-genius/top-starred-javascript-features-at-early-stage-982de948f2ae)。