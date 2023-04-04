# 现代 JavaScript 的精华——符号

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-symbols-8817986894d8?source=collection_archive---------3----------------------->

![](img/738f3a66de58dabe9d708fe08b097949.png)

由[瓦尔德马尔·布兰特](https://unsplash.com/@waldemarbrandt67w?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 符号。

# 用符号枚举自己的属性键

我们可以使用符号作为属性键。

例如，我们可以这样定义一个对象:

```
const FOO = Symbol();
const obj = {
  [FOO]: 123
};
```

然后我们可以用`Reflect.ownKeys`的方法来列举它们:

```
Reflect.ownKeys(obj)
```

`Object.keys(obj)`方法将跳过符号键。

所以它会返回`[]`。

# 作为属性键的符号

符号可以用作类实例属性。

例如，我们可以写:

```
const foo = Symbol('foo');
class Bar {
  constructor(foo) {
    this[foo] = foo;
  }
}
```

我们只是将符号传递到方括号中，将它们用作属性。

当我们在其他地方引用这些键时，我们可以以同样的方式使用它们。

# 元级属性

我们也可以使用特殊符号来定义元级属性。

例如，我们可以写:

```
const obj = {
  data: ['foo', 'bar'],
  [Symbol.iterator]() {
    //···
  }
};
```

创建可迭代对象。

然后，我们可以通过编写以下代码将它与 for-of 循环一起使用:

```
for (const x of obj) {
  console.log(x);
}
```

# 将符号转换为其他基本类型

我们可以将符号转换成其他原始类型。

我们可以将符号转换成布尔值。

例如，我们可以写:

```
Boolean(sym)
```

或者

```
!sym
```

将`sym`符号转换成布尔值。

但是，我们不能把它们转换成数字。

所以:

```
Number(sym)
```

或者:

```
sym * 10
```

会给我们类型错误。

我们可以用`String`函数或`toString`方法将符号转换成字符串。

例如，我们可以写:

```
String(sym)
```

或者:

```
sym.toString()
```

但是我们不能把它强制转换成一个字符串:

```
'' + sym
```

或者:

```
`${sym}`
```

最后两个例子会给我们带来类型错误。

防止强制将防止我们意外地将它们转换为属性键。

此外，我们不能意外地将它们用作数组索引，因为它们不能转换成数字。

# 符号的包装对象

符号没有包装对象。

我们不能将`new`操作符与`Symbol`一起使用。

所以写作:

```
new Symbol()
```

将给出“TypeError: Symbol 不是构造函数”错误。

然而，我们可以通过使用`Object`函数将它转换成一个对象:

```
const sym = Symbol();
const wrapper = Object(sym);
```

那么`wrapper`将是由`typeof wrapper`返回的`'object'`类型。

# 通过`[ ]`和包装密钥访问属性

我们可以写:

```
const foo = Symbol('foo');
const obj = {
  [sym]: 'a',
  str: 'b',
};
```

将符号传递到括号中以将它们用作属性键。

这也适用于包装符号。

例如，我们可以写:

```
const foo = Symbol('foo');
const wrappedSymbol = Object(foo);
const obj = {
  [wrappedSymbol]: 'a',
  str: 'b',
};
```

`wrappedSymbol`将被展开成一个符号，因此我们可以将包装的符号传递到方括号中。

# 属性访问

使用内部操作`ToPropertyKey`获取和设置属性。

它将操作数转换为原语，并将`ToPrimitive`转换为字符串。

如果操作数是一个对象，那么使用内部方法`[@@toPrimitive]()`将其转换为原始值。

否则，使用`toString`将操作数转换为图元。

如果`toString`不可用，则使用`valueOf`。

否则，将引发 TypeError。

![](img/11d36b30005bc7b0b15c90d5bf30f263.png)

照片由 [Glen Carrie](https://unsplash.com/@glencarrie?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

符号作为对象键和类属性键很有用。