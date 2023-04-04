# 面向对象的 JavaScript —函数和对象

> 原文：<https://blog.devgenius.io/object-oriented-javascript-functions-and-objects-28be12db6d36?source=collection_archive---------3----------------------->

![](img/c05ddf2bf1acebc7191119000b78ed29.png)

约翰·西门子在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究函数和对象。

# 迭代器

迭代器可以用 JavaScript 中的生成器函数创建。

例如，我们可以写:

```
function* genFn() {
  yield 1;
  yield 2;
  yield 3;
}
```

我们有一个生成器函数，如关键字`function*`所示。

然后我们可以调用它返回一个迭代器:

```
const gen = genFn();
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
```

我们调用了`genFn`来获得一个迭代器。

然后我们在返回的迭代器上调用了`next`。

而`next`方法将返回具有`value`和`done`属性的对象。

`value`具有来自`yield`的值。

而`done`是一个布尔值，表示是否返回了所有的值。

# 生活与街区

我们不再需要在块中包含变量的权限。

我们可以使用`let`和`const`来声明块范围的变量。

所以如果我们有:

```
(function() {
  var foo = 0;
}());
console.log(foo);
```

然后控制台日志将抛出“未捕获的引用错误:foo 未定义”错误。

我们可以对块中的`let`或`const`变量做同样的事情”

```
{
  let foo = 1;
  const bar = 2;
}
```

# 箭头功能

箭头函数对于创建回调非常有用。

例如，我们经常不得不编写这样的代码:

```
$("#submit-btn").click(function(event) {
  validateForm();
  submit();
});
```

箭头函数有点短，我们不必担心函数中`this`的值。

相反，我们可以写:

```
$("#submit-btn").click((event) => {
  validateForm();
  submit();
});
```

我们可以用不同的方式编写箭头函数。他们是:

*   无参数:`() => {...}`
*   一个参数:`a => {...}`
*   多个参数:`(a,b) => {...}`

箭头函数既可以有语句块体，也可以有表达式。

我们可以使用以下语句块:

```
n => {
  return n ** 2;
}
```

我们可以有这样一个表达式:

```
n => n ** 2;
```

# 目标

对象是面向对象编程最核心的部分。

它由各种属性和方法组成。

和属性可以包含原始值或其他对象。

我们可以通过书写来创建一个对象:

```
const person = {
  name: 'james',
  gender: 'male'
};
```

一个对象用花括号括起来。

并且`name`和`gender`是属性。

密钥可以在引号中。

例如，我们可以写:

```
const person = {
  'name': 'james',
  gender: 'male'
};
```

如果属性名是 JavaScript 中的保留字之一，我们需要引用属性名。

如果它们有空格，我们也需要引用它们。

如果它们以数字开头，那么我们也需要引用它们。

用`{}`定义一个对象被称为对象文字符号。

# 元素、属性、方法和成员

数组可以有元素。

但是对象有属性、方法和成员。

对象:

```
const person = {
  name: 'james',
  gender: 'male'
};
```

只有属性。

属性是键值对。

该值可以是任何值。

方法只是带有函数值的属性。

所以如果我们有:

```
const dog = {
  name: 'james',
  gender: 'male',
  speak() {
    console.log('woof');
  }
};
```

那么`speak`就是一个方法。

# 哈希和关联数组

对象属性可以动态地添加和删除，所以我们可以将它们用作散列或关联数组。

哈希和关联数组只是键值对。

![](img/993489a0a9c9badc1024b7d88bfe6dfc.png)

由[玛丽塔·卡维拉什维利](https://unsplash.com/@maritafox?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用生成器创建迭代器，也可以用属性和方法定义对象。