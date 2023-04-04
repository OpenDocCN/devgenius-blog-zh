# 面向对象的 JavaScript——JSON vs XML 和 Singletons

> 原文：<https://blog.devgenius.io/object-oriented-javascript-json-vs-xml-and-singletons-c981f053b770?source=collection_archive---------6----------------------->

![](img/ececfb49ce9ba473c9eca54652a37460.png)

泰勒·拉斯托维奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究一些基本的编码和设计模式。

# 使用 JSON

JSON 是一种流行的轻量级数据交换格式。

它比 XML 更好，因为我们可以很容易地在 JSON 字符串和 JavaScript 对象之间转换。

JSON 字符串的一个示例包括:

```
{
  'a': 1,
  'b': 2,
  'c': 3
}
```

XML 代码类似于:

```
<?xml version="1.0" encoding="UTF-8"?>
<response>
   <name>james</name>
   <books>
      <book>foo</book>
      <book>bar</book>
      <book>baz</book>
   </books>
</response>
```

用 JavaScript 很难把 XML 代码转换成 JSON。

我们必须安装一个库来完成它。

但是我们可以用`JSON.parse`和`JSON.stringify`在 JSON 字符串和对象文字之间进行转换。

此外，包含所有开始和结束标记的 XML 代码更长。

JSON 解析和字符串化方法也可以在 PHP 等其他语言的标准库中找到。

# 高阶函数

函数式编程局限于有限的一组语言。

JavaScript 是具有一些函数式编程特性的编程语言之一。

高阶函数是函数式编程的基本特征之一。

高阶函数是以一个或多个函数作为参数或返回一个函数作为结果的函数。

JavaScript 函数可以将函数作为参数并返回函数。

它们和其他物品一样被对待。

例如，数组实例的`map`方法接受一个函数，让我们将数组条目映射到其他东西。

例如，我们可以写:

```
[1, 2, 3, 4, 5].map(a => {
  return a ** 2;
})
```

我们对每个数组条目求平方，并返回一个包含平方条目的数组。

我们传入的函数在每个条目上都被调用，返回的结果被添加到返回的数组中。

我们可以用箭头函数很容易地写出高阶函数。

例如，不写:

```
function add(x) {
  return function(y) {
    return y + x;
  };
}
const add5 = add(5);
```

我们写道:

```
const add = x => y => y + x;
const add5 = add(5);
```

它短得多，也干净得多。

在两个`add`函数中，我们接受参数并返回一个接受参数`y`的函数，用`x`相加并返回它。

# 设计模式

设计模式是组织代码的常用语言无关方式。

《四人帮》这本书是最受欢迎的关于设计模式的书。

有几种设计模式。

创造模式处理如何创建对象。

结构模式处理如何组合不同的对象来提供新的功能。

行为模式为我们提供了对象之间相互交流的方式。

# 单一模式

单一模式是一种创造性的设计模式。

在该模式中，我们只从任何种类的类中创建一个对象。

在 JavaScript 中，我们可以简单地用一个对象文字来做这件事。

例如，我们可以写:

```
const single = {};
```

创建对象文本。

![](img/408ce66d3e8de5385a05186ee6249cc5.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Toa Heftiba](https://unsplash.com/@heftiba?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们应该使用 JSON 进行通信，因为它更容易序列化和解析。

此外，高阶函数在 JavaScript 中随处可见。

我们可以用对象文字创建单例对象。