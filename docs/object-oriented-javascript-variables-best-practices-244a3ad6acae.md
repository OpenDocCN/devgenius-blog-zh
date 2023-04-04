# 面向对象的 JavaScript —变量最佳实践

> 原文：<https://blog.devgenius.io/object-oriented-javascript-variables-best-practices-244a3ad6acae?source=collection_archive---------10----------------------->

![](img/4eb6ded59f532bd7eed7ba858fdc6864.png)

照片由[法希姆](https://unsplash.com/@fahimhasan999?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将看看变量的范围。

# 变量的范围

从 ES6 开始，JavaScript 就有了块范围的变量。

只要我们用`let`或`const`声明变量，它们将是块范围的。

我们可以写:

```
function foo() {
  let local = 2;
}
```

创建一个块范围的变量`local`。

这仅在街区内可用。

我们也可以创建全局变量。

例如，我们可以写:

```
var x = 1;
```

在顶层创建一个全局变量。

它是提升的，这意味着它的声明可以从脚本中的任何地方访问。

但是该值只有在赋值后才可用。

`var`用于顶级将是全球性的。

我们应该避免创建全局变量以避免命名冲突。

块范围的变量更容易跟踪，因为它们只在块内可用。

# 可变提升

变量提升只能通过`var`变量完成。

例如，我们可以写:

```
var a = 123;
```

那么如果我们如下使用它:

```
var a = 123;function foo() {
  console.log(a);
  var a = 1;
  console.log(a);
}
foo();
```

然后我们得到`a`的第一个日志是`undefined`。

第二个控制台日志是 1。

这是因为控制台日志从函数中获取了`a`。

`var`是否为函数作用域，其值将取自函数。

因此，我们分别得到`undefined`和 1。

# 块范围

因为函数范围令人困惑，ES6 引入了块范围的变量。

我们可以用`let`和`const`来声明变量。

它们没有被提升，并且`const`变量在声明时必须被赋予一个初始值。

例如，我们可以写:

```
var a = 2; {
  let a = 3;
  console.log(a); 
}
console.log(a);
```

控制台日志中的`a`应该是 3。

块外的控制台日志将记录来自`var`声明的 2。

因为使用块范围的变量更容易推理，所以我们应该在任何地方使用它们。

创建变量的规则是我们首先考虑`const`我们不能给它们赋值。

如果我们需要给一个变量赋值，那么我们使用`let`。

而且我们从来不用`var`。

# 函数是数据

函数可以分配给变量。

例如，我们可以写:

```
const foo = function() {
  return 1;
};
```

然后我们给`foo`变量分配了一个函数。

这叫做函数表达式。

如果我们写下:

```
const foo = function bar() {
  return 1;
};
```

然后我们给一个变量赋值一个函数声明。

它们是相同的，除了我们可以在函数中访问原始名称。

所以我们可以从`bar`函数体中得到名字`bar`。

但是我们叫它外面有`foo`。

使用`typeof`操作符，我们返回类型`'function'`作为值。

如果我们有:

```
typeof foo
```

我们得到`'function'`。

![](img/4842a01374be8ea7ce3f1065e1a96f1a.png)

照片由 [Hannah Busing](https://unsplash.com/@hannahbusing?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们应该使用`let`或`const`来声明变量。

此外，函数可以分配给变量。