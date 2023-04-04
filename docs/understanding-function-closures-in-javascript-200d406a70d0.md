# 理解 JavaScript 中的函数闭包

> 原文：<https://blog.devgenius.io/understanding-function-closures-in-javascript-200d406a70d0?source=collection_archive---------3----------------------->

## JavaScript 中的函数闭包及实例

![](img/dfb4a85c02ca45f7204148f0c0d38ef6.png)

照片由 [Arif Riyanto](https://unsplash.com/@arifriyanto?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 什么是终结？

闭包是每个 JavaScript 开发人员都应该知道的概念。它允许您从内部函数访问外部函数的范围。在 JavaScript 中，闭包是在每次创建函数时创建的。准确理解闭包将有助于您编写更好、更高效、更干净的代码。

在本文中，我们将通过实际例子来探索 JavaScript 中的函数闭包。让我们开始吧。

![](img/ac9a6378293e3ba100322fa66df3beea.png)

由[迈赫迪·奥西德](https://mehdiouss315.medium.com/)与❤️️一起创作的图像。

# 全局和局部变量

JavaScript 变量可以属于局部或全局范围。然而，全局变量可以通过**闭包**成为局部(私有)变量。

一个`function`可以访问函数中定义的所有变量，就像这样:

```
function myFunction() {
  var a = 4;
  return a * a;  //16.
}
```

它还可以访问函数外部定义的变量。看看下面的例子:

```
var a = 4;
function myFunction() {
  return a * a;   //16.
}
```

在第一个例子中，变量`a`是局部的，它只能在定义它的函数内部使用。另一方面，第二个例子中的变量是全局的，它可以被页面上(和窗口中)的所有脚本使用(和更改)。

# 可变寿命

全局变量一直存在，直到页面被丢弃，就像当您导航到另一个页面或关闭窗口时。

局部变量的寿命很短。它们是在调用函数时创建的，在函数完成时被删除。

# 一个反困境的例子

假设你想用一个变量来计数，你想让这个计数器对所有函数都可用。你可以使用一个全局变量和一个`**function**`来增加计数器:

```
// Initiate counter
var counter = 0;

// Function to increment counter
function add() {
  counter += 1;
}

// Call add() 3 times
add();
add();
add();

// The counter should now be 3
```

但这里的问题是，外面的任何代码都可以改变计数器，而不需要调用`**add()**`。计数器应该在`add()`函数的本地，以防止其他代码改变它。看看下面的例子:

```
// Initiate counter
var counter = 0;

// Function to increment counter
function add() {
  var counter = 0;
  counter += 1;
}

// Call add() 3 times
add();
add();
add();

//The counter should now be 3\. But it is 0
```

这是行不通的，因为我们显示的是全局计数器，而不是本地计数器。我们可以移除全局计数器，并通过让函数返回它来访问局部计数器。

看看这个例子:

```
// Function to increment counter
function add() {
  var counter = 0;
  counter += 1;
  return counter;
}

// Call add() 3 times
add();
add();
add();

//The counter should now be 3\. But it is 1.
```

它没有再次工作，因为我们每次调用该函数时都会重置本地计数器。一个 JavaScript **内部函数**可以解决这个问题。

在下面的例子中，内部函数`plus()`可以访问父函数中的变量`counter`:

```
function add() {
  var counter = 0;
  function plus() {counter += 1;}
  plus();   
  return counter;
}
```

如果我们能从外部接触到`plus()`功能，这可能已经解决了计数器的难题。

我们还需要找到一种只执行一次`counter =0`的方法。因此，这里我们需要一个 JavaScript **闭包**。

# JavaScript 闭包

看看下面的例子:

```
var add = (function () {
  var counter = 0;
  return function () {counter += 1; return counter}
})();

add();
add();
add();

// the counter is now 3
```

在上面的例子中，变量`add`被分配给一个自调用函数的返回值。自调用函数只运行一次。它将计数器设置为零(0)并返回一个函数表达式。这样，add 就变成了一个函数，它可以访问父作用域中的计数器。

这被称为 JavaScript **闭包。**它使得函数拥有“私有”变量成为可能。计数器受匿名函数的作用域保护，只能使用 add 函数进行更改。

# 结论

闭包只是一个可以访问父作用域的函数，即使在父函数关闭之后。此外，我们使用闭包主要是为了在 JavaScript 中实现封装、迭代器和 singleton。这只是理解函数闭包的一个简单介绍，你需要从其他资源中学习更多。

感谢您阅读这篇文章。

# 更多阅读

[](https://medium.com/javascript-in-plain-english/5-javascript-functional-programming-concepts-that-you-should-know-c96f14b02a87) [## 你应该知道的 5 个 JavaScript 函数式编程概念

### 带有实际例子的函数式编程概念

medium.com](https://medium.com/javascript-in-plain-english/5-javascript-functional-programming-concepts-that-you-should-know-c96f14b02a87)