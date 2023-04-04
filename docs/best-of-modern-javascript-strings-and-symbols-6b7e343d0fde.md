# 现代 JavaScript 的精华——字符串和符号

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-strings-and-symbols-6b7e343d0fde?source=collection_archive---------10----------------------->

![](img/31146da9362e0d9fd2ce2e42ef6ce79d.png)

照片由 [Olivier Collet](https://unsplash.com/@ocollet?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# 新的字符串方法

新的字符串方法包括代码点和模板文字方法。

有一个`String.raw`方法返回一个字符串的原始版本。

例如，我们可以写:

```
String.raw`\n`
```

返回字符串的原始版本。

斜线没有被解释。

用于模板文字的函数称为模板标签。

`String.fromCodepoint`方法从给定的代码点返回一个字符串。

例如，我们可以这样使用它:

```
String.fromCodePoint(9731, 9733, 9842)
```

然后我们得到:

```
"☃★♲"
```

`String.prototype.normalize`方法让我们将具有相同值的代码点组合组合在一起。

例如，我们可以写:

```
'\u00F1'.normalize('NFC')
```

然后我们得到:

```
"ñ"
```

`'NFC'`是规范化字符串的格式，也是默认的格式。

这意味着规范分解之后是规范合成。

# 标志

符号是 ES6 的一个新的基本类型。

我们可以用`Symbol`工厂函数来创建它。

例如，我们可以写:

```
const foo = Symbol('foo');
```

每次我们给工厂打电话，都会产生一个新的符号。

所以没有两个符号是相同的。

这些作为唯一属性键很有用。

例如，`Symbol.iterator`符号是用于创建可迭代对象的特殊符号。

我们可以写:

```
const iterableObject = {
  [Symbol.iterator]() {
    //···
  }
}
for (const x of iterableObject) {
  console.log(x);
}
```

创建对象并用 for-of 循环遍历它。

我们也可以用它们作为常数来表示概念。

例如，我们可以写:

```
const COLOR_RED = Symbol('red');
const COLOR_GREEN = Symbol('green');
const COLOR_BLUE = Symbol('blue');function getValue(color) {
  switch (color) {
    case COLOR_RED:
      return 1;
    case COLOR_BLUE:
      return 2;
    case COLOR_GREEN:
      return 2;
    default:
      throw new Error('Unknown color: ');
  }
}
```

我们必须使用从`Symbol`创建的变量来引用一个符号，因为每次我们调用`Symbol`时都会创建一个新的符号。

符号不能被强制为字符串。

例如，以下内容不起作用:

```
const foo = Symbol('foo');
const str = foo + '';
```

或者

```
const foo = Symbol('foo');
const str = `${foo}`;
```

在这两个示例中，我们都将得到错误“未捕获的类型错误:无法将符号值转换为字符串”。

我们可以用`toString`来转换。

所以我们可以写:

```
const foo = Symbol('foo');
const str = foo.toString();
```

那么`str`就是`'Symbol(foo)’`。

以下操作可识别符号:

*   `Reflect.ownKeys()`
*   通过`[]`的属性访问
*   `Object.assign()`

以下忽略了作为键的符号:

*   `Object.keys()`
*   `Object.getOwnPropertyNames()`
*   `for-in`循环

`Symbol`的字符串参数是可选的。

一切符号都是独一无二的，所以:

```
Symbol() === Symbol()
```

返回`false`。

如果我们使用`typeof`操作符:

```
typeof Symbol()
```

我们得到`'symbol'`。

我们可以通过编写以下代码将符号用作属性键:

```
const FOO = Symbol();
const obj = {
  [FOO]: 123
};
```

我们也可以用它来定义方法:

```
const FOO = Symbol();
const obj = {
  [FOO](){}
};
```

![](img/32a5b37944b4a503e306e28d1bde63b6.png)

照片由 [Amrit Sangar](https://unsplash.com/@byamrit?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

ES6 可以正确识别所有 Unicode 字符。

此外，它还引入了用作唯一标识符的符号数据类型。