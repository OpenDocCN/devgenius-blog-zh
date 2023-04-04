# Javascript 箭头函数

> 原文：<https://blog.devgenius.io/javascript-arrow-function-7493fbd892f3?source=collection_archive---------5----------------------->

# 概观

箭头函数类似于 Java 和 Pythons 的 lambda 函数。可以用箭头函数代替 Javascript 传统的函数定义。箭头函数更紧凑，但也有一些限制:

*   没有到这个或*超级*的绑定，所以如果它们是方法，它不应该在对象内部使用
*   无法访问*参数*或*新目标*
*   不应与*调用*、*应用*和*绑定*方法一起使用，因为它们依赖于*范围*或正在执行的当前上下文
*   不能用作构造函数
*   不能在其定义中使用*产量*

![](img/26c2cdb39195c0795426d1dfc54c41ed.png)

照片由[尼克·费因斯](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 履行

您可以像往常一样将参数放在括号中，而不是箭头= >，然后是定义，而不是使用功能键。如果只有一个参数，可以忽略括号。类似地，如果定义只有一行，你可以忽略花括号和 return 关键字，因为它是隐含的。您也可以将 arrow 函数赋给变量。

```
// Traditional function
function square(x) {
  return x * x
}// Arrow function
(x) => { 
  return x * x
}// Arrow function without parenthesis, single argument
x => {
  return x * x
}// Arrow function without parenthesis and curly braces, single argument and one line of code with implied return
x => x * x// Assigning arrow function
const squareArrow = x => x * x
```

如果 arrow 函数没有参数，您仍然需要括号，就像有多个参数一样。

```
// No argument
const x = () => 'x'// Multiple arguments
const adder = (x, y) => x + y
```

# 结论

箭头函数是匿名的，这使得它们比传统函数更难调试。像 Java 和 Pythons lambda 一样，它们经常被用在高阶函数中。当使用 Javascript 内置的高阶函数时，比如 *find* 、 *map* 、 *filter* ，你可以适当地使用箭头函数。