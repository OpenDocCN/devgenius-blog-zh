# 面向对象的 JavaScript —循环和函数

> 原文：<https://blog.devgenius.io/object-oriented-javascript-loops-and-functions-be1d303e2ac5?source=collection_archive---------7----------------------->

![](img/4b6391365bb306e06701578967fd5565.png)

艾伦·尼格伦在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究各种循环和函数。

# 环

我们可以在 JavaScript 中使用各种循环。

## 对于…循环

for…in 循环让我们遍历对象的键。

例如，我们可以写:

```
let obj = {
  a: 1,
  b: 2,
  c: 3
}
for (const key in obj) {
  console.log(key, obj[key])
}
```

我们用密钥得到了`key`，我们可以用`obj[key]`访问这个值。

## 对于…的循环

for…of 循环让我们遍历可迭代对象，包括数组。

例如，我们可以写:

```
let arr = [1, 2, 3]
for (const a of arr) {
  console.log(a);
}
```

我们有`arr`数组，我们用 for-of 循环遍历它。

`a`有数组条目。

# 评论

注释是被 JavaSdcript 引擎忽略的代码的一部分。

我们可以用它们来解释我们的代码是如何工作的。

单行注释从`//`开始，到行尾结束。

多行注释以`/*`开始，以`*/`结束。

它们之间的任何内容都将被忽略。

所以我们可以写:

```
// single line
```

并且:

```
/* multi-line comment on a single line */
```

或者:

```
/* 
  multi-line comment on a single line 
*/
```

# 功能

函数是一段可重用的代码。

我们可以用它们在我们的代码中做一些事情。

有各种各样的功能，它们包括:

*   匿名函数
*   复试
*   即时(自调用)函数
*   内部函数(在其他函数中定义的函数)
*   返回函数的函数
*   重新定义自己的功能
*   关闭
*   箭头功能

他们让我们将代码组合在一起，给它一个名字，并在以后重用它。

我们可以这样定义一个函数:

```
function sum(a, b) {
  let c = a + b;
  return c;
}
```

我们在函数中返回`a`和`b`的和。

`a`和`b`是参数，我们返回值。

我们用关键字`function` 来定义一个函数。

让我们返回一个值。

如果没有`return`语句，那么它返回`undefined`。

我们也可以通过编写来创建箭头函数

```
const sum = (a, b) => {
  let c = a + b;
  return c;
}
```

或者我们可以写:

```
const sum = (a, b) => a + b
```

它们不与`this`绑定，而且更短。

# 调用函数

我们可以通过编写以下代码来调用函数:

```
const result = sum(1, 2);
```

括号中的值是参数。

它们被设置为参数的值。

# 因素

参数是函数括号中的项目。

所以如果我们有:

```
function sum(a, b) {
  let c = a + b;
  return c;
}
```

那么`a`和`b`就是括号。

我们传入参数来设置它们。

它们是由位置决定的。

如果我们忘记传递它们，那么它们将会是`undefined`。

JavaScript 在检查参数时并不挑剔。

类型可以是任何东西，我们可以跳过其中的任何一个。

它只会做一些我们没有预料到的事情，如果我们没有传入预期的内容，它就会无声地失败。

![](img/3ec544c322be8ef81349f71c3b7f7d02.png)

照片由[亚历克斯·马查多](https://unsplash.com/@alexmachado?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

函数是我们可以调用的可重用代码。

for…in 循环让我们遍历对象，for…of 循环让我们遍历可迭代对象。