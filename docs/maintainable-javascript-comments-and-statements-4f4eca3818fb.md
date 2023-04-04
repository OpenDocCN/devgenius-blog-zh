# 可维护的 JavaScript——注释和语句

> 原文：<https://blog.devgenius.io/maintainable-javascript-comments-and-statements-4f4eca3818fb?source=collection_archive---------13----------------------->

![](img/eae7e3aee41b0d08635e72b9e2f023e9.png)

照片由[珍·西奥多](https://unsplash.com/@jentheodore?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过适当地编写注释和语句来了解创建可维护的 JavaScript 代码的基础。

# 潜在的作者错误

如果代码中有作者错误，那么我们应该对它们进行注释，以便可以修复它们。

至少有人可以检查这是否是一个错误。

例如，如果我们有:

```
while (element && (element = element[axis])) {
  // NOTE: assignment
  if ((all || element[TAG_NAME]) &&
    (!fn || fn(element))) {
    return element;
  }
}
```

那么可能有人会认为`while`循环头中的赋值操作是错误的。

这是一个有效的语法，所以我们可以使用它。

这不是一个错误，所以我们告诉读者，这不是一个评论。

它可能被一些林挺工具标记，因为我们通常不在循环头中使用赋值。

# 针对浏览器的黑客攻击

如果我们需要让我们的代码在多种浏览器下工作，那么我们可能需要编写代码来让它们在特定的浏览器下工作。

当我们需要让我们的前端代码与 Internet Explorer 一起工作时，这是很常见的。

如果我们的代码中有黑客，我们应该对我们正在做的事情做出评论，这样人们就知道发生了什么。

随着浏览器越来越标准化，这种情况应该会越来越少。

# 文档注释

注释可以用于 JSDoc 等程序的文档。

例如，我们可以写:

```
/**
  add 2 numbers together
  @method add
  @param {number} x - a number 
  @param {number} y - another number 
  @return {number} the sum of 2 numbers
**/
function add(x, y) {
  return x + y;
}
```

我们有一个有两个参数的`add`函数。

我们添加一些关于参数的注释，这样我们就知道参数的类型了。

同样，我们对返回类型也做了同样的处理。

这可以用文档生成器读取，以创建我们的文档。

我们应该记录所有的方法，包括预期的参数和可能的返回值。

构造函数应该有带有自定义类型和预期参数的注释。

拥有一个或多个方法的其他对象可以对这些方法进行注释。

必须对它们进行记录，以便正确生成文档。

文档生成器将确定注释应该如何格式化。

# 声明

像`if`或`for`这样的语句在 JavaScript 中有两种用法。

我们可以有花括号，或者如果我们的代码块只有一行那么我们可以跳过它们。

然而，跳过花括号是不好的，因为很难区分块的开始和结束。

所以与其写:

```
if (condition)
  doWork();
```

或者

```
if (condition) doWork();
```

或者

```
if (condition) { doWork(); }
```

我们应该写:

```
if (condition) {
  doWork();
}
```

把身体放在第一条线以下比放在同一条线上好。

花括号包围着这个块，很容易区分这个块在哪里开始或结束。

![](img/712621cd574df0cae62d101b7712b930.png)

詹尼斯·卢卡斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

应该为潜在的错误或难以理解的代码添加注释。

另外，语句应该用花括号括起来。