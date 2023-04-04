# 可维护的 JavaScript —声明和函数调用

> 原文：<https://blog.devgenius.io/maintainable-javascript-declarations-and-function-calls-93f3064d3e58?source=collection_archive---------5----------------------->

![](img/4944634064f6b60f70a59a86855d7419.png)

克里斯·蒙哥马利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看变量和函数来了解创建可维护 JavaScript 代码的基础。

# 变量声明提升

`var`语句被挂起。

这意味着声明可以在定义之前被访问。

这是因为声明被拉到顶部而没有值。

我们不希望这种情况自动发生，因为这会让我们困惑。

幸运的是，有块范围的`let`和`const`变量，它们没有被提升。

这很好，因为我们不会被变量在代码中的位置所迷惑。

为了使我们的生活更容易，我们应该把变量作为函数中的第一个语句。

一个函数不应该太长，这样我们就不必跳得太远。

例如，我们可以写:

```
function doWork(items) {
  let i;
  const len = items.length;
  let value = 10;
  let result = value + 10;
  for (i = 0; i < len; i++) {
    console.log(items[i]);
  }
}
```

我们到处使用`let`和`const`。

与`for`循环一样，项目被迭代。

`len`用`const`声明，因为它从不改变。

道格·克罗克福德也推荐了这种风格。

我们还将所有变量声明合并为每行一个值。

# 函数声明

函数声明也像变量声明一样被 JavaScript 引擎挂起。

所以我们可以在声明之前在代码中使用函数声明。

例如，我们可以写:

```
doSomething();function doSomething() {
  console.log("hello world");
}
```

我们在声明函数之前调用了`doSomething`。

这是因为提升的缘故。

然而，为了减少混乱，最好在使用函数之前声明它们。

所以我们应该写:

```
function doSomething() {
  console.log("hello world");
}doSomething();
```

相反。

ESLint 和 JSHint 会在函数被声明之前就发出警告。

函数声明不应出现在 block 语句中。

例如，我们不应该编写这样的代码:

```
if (condition) {
  function doSomething() {
    console.log("foo");
  }
} else {
  function doSomething() {
    console.log('bar')
  }
}
```

这不是有效的语法，但是仍然被一些浏览器使用。

因此，这是 ES 规范中应该避免的灰色区域。

函数声明只能在条件语句之外使用。

这在谷歌风格指南中也是禁止的。

# 函数调用间距

函数调用间距是一个没有太大争议的问题。

大多数人认为函数调用在函数名和左括号之间不应该有空格。

这样做是为了与 block 语句区分开来。

例如，我们应该写:

```
doWork(item);
```

而不是像这样:

```
doWork (item);
```

这是为了将其与块语句区分开来，例如:

```
while (item) {
  // ...
}
```

道格·克罗克福德的代码惯例指出了这一点。

谷歌的风格指南也推荐这种风格。

# 即时函数调用

JavaScript 允许我们声明没有属性名的匿名函数，并将这些函数赋给变量和属性。

我们可以写:

```
const doSomething = function() {
  // ...
};
```

我们将匿名函数赋给了一个变量，所以我们可以用变量名调用它。

调用它时函数本身没有括号是不好的。

例如，我们不应该写:

```
const value = function() {
  //...
  return {
    message: "hello"
  }
}();
```

很容易将它与定义函数并将其赋给变量而不调用它混淆。

![](img/b6da795f96f2e51b187df21c127bc469.png)

由[柏克莱通信](https://unsplash.com/@berkeleycommunications?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

`var`和函数声明被吊起。

所以它们不应该被使用。

此外，我们不应该使用不带括号的直接函数调用。