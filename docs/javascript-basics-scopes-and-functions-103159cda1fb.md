# JavaScript 基础—作用域和函数

> 原文：<https://blog.devgenius.io/javascript-basics-scopes-and-functions-103159cda1fb?source=collection_archive---------26----------------------->

![](img/c4df8ba4e784dfbc6fd3aaea81e2d6c6.png)

[Kasia Wanner](https://unsplash.com/@tueio?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 是世界上最流行的编程语言之一。为了有效地使用它，我们必须了解它的基本知识。

在本文中，我们将看看变量的作用域，以及函数的定义和调用。

# 嵌套作用域

块和函数可以在 JavaScript 程序中创建自己的变量范围。

例如，我们可以写:

```
const fn = () => {
  let x = 1;
}
```

那么`x`在块内是可用的。

我们也可以有函数范围的`var`变量。

例如，我们可以写:

```
const fn = () => {
  var x = 1;
}
```

那么函数中也有变量。

如果我们在另一个程序块中有变量，比如一个`if`程序块。

例如，我们可以写:

```
const fn = () => {
  let x = 1;
  if (true) {
    let y = 1;
  }
}
```

那么`y`在`if`街区之外就不可用了。

另一方面，如果我们有:

```
const fn = () => {
  let x = 1;
  if (true) {
    var y = 1;
  }
}
```

则`y`可从`if`块外部获得。

# 作为值的功能

函数可以做其他值可以做的所有事情。

我们可以在表达式中使用它们，而不仅仅是调用它。

我们可以将函数存储在一个变量中，并且可以将它们作为参数传入。

例如，我们之前的函数:

```
const fn = () => {
  let x = 1;
}
```

被分配给`fn`常量。

# 声明符号

我们可以用更短的方式编写函数。

我们可以使用声明符号，如下所示:

```
function cube(x) {
  return x ** 3;
}
```

该语句定义了`cube`函数，并且在其后不需要分号。

我们可以在它们被定义之前使用它们。

例如，我们可以写:

```
const cubed = cube(2);function cube(x) {
  return x ** 3;
}
```

在它被定义之前我们称之为。

这是因为它会自动被拉到顶部。

函数声明不是常规的自顶向下流程控制的一部分。

# 箭头功能

箭头函数就是我们之前看到的。它们由`=>`箭头表示。

箭头由带大于号的等号组成。

箭头位于参数列表之后，后面是函数体。

例如，我们可以这样定义它:

```
const cube = (x) => {
  return x ** 3;
}
```

或者:

```
const cube = (x) => x ** 3;
```

它们是相同的，因为它们都隐式或显式地返回相同的值。

第一个示例使用关键字`return`显式返回，第二个示例隐式返回。

箭头函数用于以更短的方式编写 JavaScript 函数。

![](img/77a68cee6cb7e50b8378bdf9f530ce1b.png)

[亚历克斯·哈维拍摄🤙🏻](https://unsplash.com/@alexharvey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 调用栈

调用堆栈是为获得结果而调用的所有函数的记录。

它存储在计算机的内存中。当堆栈变得太大时，计算机将抛出一个错误。

例如，如果我们有:

```
const a = () => {
  console.log('a');
}const b = () => {
  console.log('b');
  a();
}const c = () => {
  console.log('c');
  b();
}c();
```

然后我们得到:

```
c
b
a
```

来自控制台日志。

函数序列是所调用函数的堆栈，按照它们被调用的顺序排列。

它从最晚到最早。

# 可选参数

函数参数不必与函数的参数匹配。

例如，我们可以将`cube`称为:

```
const cubed = cube(2, 'foo');
```

它需要一个参数，但我们传入了两个参数。

我们可以将任何东西传递给 JavaScript 函数。不检查函数调用。

多余的将被丢弃。

如果我们没有传入足够的参数来设置所有参数，那么那些没有传入的参数将被设置为`undefined`。

例如，如果我们这样称呼`cube`:

```
const cubed = cube(2);
```

`cubed`为`NaN`，而`x`为`undefined`。

# 结论

变量的嵌套范围取决于如何声明变量以及它们位于何处。

我们可以用几种方法来定义函数。我们可以定义函数声明或箭头函数。

调用堆栈是所调用函数的记录。