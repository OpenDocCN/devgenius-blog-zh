# 有用的 JavaScript 技巧——格式化数字和对象

> 原文：<https://blog.devgenius.io/useful-javascript-tips-formatting-numbers-and-objects-498357062dcf?source=collection_archive---------42----------------------->

![](img/8bd227529e678cacfa5d0061acd410d8.png)

惠特尼·赖特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 国际机场。数字格式

我们可以使用`Intl.NumberFormat`构造函数来格式化数字。

例如，我们可以写:

```
const formatter = new Intl.NumberFormat('fr-ca', {
  style: 'currency',
  currency: 'CAD'
})
const price = formatter.format(100);
```

那么`price`就是`“100,00 $”`，因为我们将地区设置为加拿大法语，货币是加元。

我们还可以设置`minimumFractionDigits`属性来设置返回数字字符串中的最小小数位数。

例如，我们可以写:

```
const formatter = new Intl.NumberFormat('fr-ca', {
  style: 'currency',
  currency: 'CAD',
  minimumFractionDigits: 2
})
const price = formatter.format(100);
```

我们将`minimumFractionDigits`设置为 2，这样我们将总是显示这些数字。

# 国际机场。多重规则

`Intl.PluralRules`构造函数让我们返回要使用的复数规则的名称。

例如，我们可以写:

```
const pr = new Intl.PluralRules('en-gb', {
  type: 'ordinal'
})
```

然后我们可以对`pr`使用`select`方法，如下所示:

```
pr.select(0)
```

然后我们得到`'other'`。如果我们写`pr.select(1)`，我们得到`'one'`。

`pr.select(2)`返回`'two'`，`pr.select(3)`返回`'few'`。

现在，我们可以创建一个对象来获取该语言的正确序号后缀。

例如，我们可以写:

```
const suffixes = {
  'one': 'st',
  'two': 'nd',
  'few': 'rd',
  'other': 'th'
}
```

因此，我们可以将数字格式化为:

```
const format = number => `${number}${suffixes[pr.select(number)]}`
```

然后我们可以用适当的数字调用`format`函数:

```
format(0)
```

然后我们得到`'0th'`。`format(1)`返回`'1st'`以此类推。

# 目标

创建对象有很多方法，我们可以创建一个对象文字，使用`Object`构造函数或者使用`Object.create`。

例如，我们可以写:

```
const dog = {};
```

或者:

```
const dog = Object();
```

或者:

```
const dog = Object.create();
```

`Object.create`用于创建一个具有自己原型的对象。

如果我们不需要设置自己的原型，对象文字是首选方式。

我们还可以创建一个构造函数来创建多个相同类型的对象。

例如，我们可以写:

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName= lastName;
}
```

然后我们可以通过运行以下命令来使用`new`操作符:

```
const person = new Person('james', 'smith');
```

对象总是通过引用传递。

如果我们把它赋给一个变量，那么这个变量就引用原始对象。

# 内置对象属性

大多数对象都有内置属性。

它有一个包含`Object.prototype`对象的原型，大多数对象都是从这个原型继承而来的。

所有不是用`null`原型创建的对象都继承自`Object.prototype`。

# ()方法的对象值

JavaScript 对象有`valueOf`方法来返回对象的原始表示。

例如，我们可以写:

```
const person = { name: 'joe' };
const val = person.valueOf();
```

然后我们拿回物体。

然而，它的价值在于我们可以重写方法来返回我们想要的结果。

# Object toString()方法

`toString`方法返回一个对象的字符串表示。

例如，我们可以写:

```
const person = { name: 'joe' };
const val = person.valueOf();
```

然后我们得到了`‘[object Object]'`，但是我们可以覆盖它来得到我们想要的字符串值。

# 检查对象是否可枚举

对象的可枚举性表明我们是否可以遍历一个对象属性。

例如，我们可以写:

```
const person = { name: 'james' }Object.defineProperty(person, 'age', {
  value: 27,
  enumerable: false
})
```

然后我们可以使用`propertyIsEnumerable`方法来检查属性是否可以被遍历。

例如，我们可以写:

```
person.propertyIsEnumerable('name')
```

并返回`true`。

并且:

```
person.propertyIsEnumerable('age')
```

返回`false`。

因此，如果我们使用 for-in 循环遍历`person`，那么我们将看到`name`而不是`age`。

![](img/5c73bc4e2e6b1abc87bee2189f655c92.png)

照片由[克里斯·桑蒂利](https://unsplash.com/@nomadicrx_?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用对象文字、`Object`构造函数和`Object.create`方法创建对象。

要检查一个对象属性是否是可枚举的，那么我们可以使用`propertyIsEnumerable`方法。

还有一些以语言敏感的方式格式化数字的方法。