# JavaScript 最佳实践—空间、对象和字符串

> 原文：<https://blog.devgenius.io/javascript-best-practices-spaces-objects-and-strings-7b9bcdea342f?source=collection_archive---------22----------------------->

![](img/d1802f8e329876d9e3c18c4f5550c843.png)

照片由[autthaporate pradid pong 先生在](https://unsplash.com/@autthaporn?utm_source=medium&utm_medium=referral)[un plash](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种问题。

在本文中，我们将研究在编写 JavaScript 代码时应该遵循的一些最佳实践。

# 没有用于缩进的混合空间和制表符

混合空格和制表符会导致文本编辑器出现问题。

因此，我们不应该将它们混合在一起。

相反，我们使用 2 个空格，并自动将制表符转换为 2 个空格。

# 除缩进外，没有多个空格

我们只使用 2 个空格作为缩进。

在所有其他地方，我们应该使用一个空间，以避免浪费空间。

# 没有将返回对象分配给变量就没有新的

如果我们用`new`创建一个新的对象，那么我们应该把它分配给一个变量，这样我们就可以使用它了。

例如，不写:

```
new Dog();
```

我们写道:

```
const dog = new Dog();
```

# 不要使用函数构造函数

我们不应该使用`Function`，它接受函数代码的字符串并返回一个函数。

在字符串中运行代码是一种安全隐患。

此外，因为代码在字符串中，所以很难调试和优化。

例如，我们不应该写:

```
const add = new Function('a', 'b', 'return a + b');
```

相反，我们写道:

```
const add = (a, b) => a + b;
```

# 不要使用对象构造函数

我们不应该使用`Object`构造函数，因为它不会给我们带来用对象文字创建它们的好处。

这只会让代码更长。

例如，不写:

```
let foo = new Object();
```

我们应该写道:

```
let foo = {};
```

# 没有新的要求

我们不应该把`new`和`require`放在一起。

这可能会造成以下两者之间的混淆:

```
const foo = new require('foo');
```

以及:

```
const foo = new (require('foo'));
```

因此，我们应该避免这些表达。

# 不要将符号作为构造函数

`Symbol`为出厂功能。它不是构造函数。

因此，我们不应该写:

```
const foo = new Symbol('foo');
```

相反，我们写道:

```
const foo = Symbol('foo');
```

# 没有原始包装实例

我们不应该使用像`String`或`Boolean`这样的函数作为构造函数。

这是因为它们返回类型为`'object'`的值，这很令人困惑。

此外，代码更长。

因此，我们不应该使用它。

例如，不写:

```
const message = new String('hi');
```

我们写道:

```
const message = 'hi';
```

文字更短，可以消除任何可能出现的混淆。

# 不要将全局对象属性称为函数

我们不应该把全局对象属性称为函数。

他们不应该被召唤。

例如，我们不应该写:

```
const math = Math();
```

# 无八进制文字

我们不应该写八进制数字文字。

它们在 JavaScript 中几乎从来没有用处，因为它们以 0 开头，所以我们可能会将其与十进制数字混淆。

例如，我们不应该写:

```
const octal = 042;
```

相反，我们写道:

```
const decimal = 34;
```

# 没有八进制转义序列

我们的代码中不应该有八进制转义序列。

它们没有用，拥有它们可能是个错误。

例如，我们不应该写:

```
const foo = 'foo \251'
```

相反，我们写道:

```
const foo = 'foo';
```

# 使用 __dirname 或 __filename 时没有字符串串联

我们应该使用`path.join`来连接路径，这样我们就可以在所有操作系统中正确地使用路径。

例如，代替书写:

```
const pathToFile = __dirname + '/foo.js'
```

我们写道:

```
const pathToFile = path.join(__dirname, 'foo.js')
```

# 不要使用 __proto__

如`__`字符所示，`__proto__`属性并不意味着被直接访问。

相反，我们应该使用`Object.getPrototypeOf`方法来获取对象的原型。

所以我们不应该写:

```
const foo = obj.__proto__;
```

但是我们应该写:

```
const foo = Object.getPrototypeOf(obj);
```

# 不要重新声明变量

我们不应该在代码中重新声明变量。

如果我们在同一个作用域中声明了两个同名的变量，我们会得到一个错误。

相反，我们应该给现有的同名变量重新赋值。

例如，不要写:

```
let name = 'james';
let name = 'joe';
```

我们写道:

```
let name = 'james';
name = 'joe';
```

![](img/7073e72dfee5fc4a8b95eff92fe159a3.png)

照片由[贝扎德·加法尔安](https://unsplash.com/@behz?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们不应该在同一个范围内声明同名的变量。

同样，我们应该用`getPrototypeOf`的方法得到一个物体的原型。

我们还应该注意 JavaScript 代码的间距。