# 现代 JavaScript 的精华— let 和 const

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-let-and-const-c49d19751753?source=collection_archive---------14----------------------->

![](img/f2896a3edc00842d995b3fc1aa34f25b.png)

约翰·邓肯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 变量。

# `const`创建不可变变量

我们可以用`const`创建不可变变量。

例如，如果我们有:

```
const foo = 'abc';
foo = 'baz';
```

然后我们有一个类型错误。

# `const`并不使价值不可变

即使变量不能被重新分配，它的值仍然可以改变。

例如，如果我们有一个对象:

```
const obj = {};
```

我们可以给它添加属性:

```
const obj = {};
obj.prop = 'foo';
console.log(obj.prop);
```

`obj.prop`会是`'foo'`。

如果我们想让对象不可变，我们可以调用`Object.freeze`方法:

```
const obj = Object.freeze({});
```

`Object.freeze`仅阻止顶级改变。

存储在其属性中的对象仍然是可变的。

如果我们有:

```
const obj = Object.freeze({ foo: {} });
```

然后我们可以写:

```
obj.foo.qux = 'baz';
```

这是可行的。

# `const`在循环体中

我们可以在循环体中使用`const`。

例如，我们可以写:

```
function logArgs(...args) {
  for (const [index, elem] of args.entries()) { 
    console.log(index, elem);
  }
}
```

我们调用`entries`方法，该方法返回带有条目索引的`index`和带有条目本身的`elem`的条目。

`const`防止数组赋值。

# `var`声明变量的生命周期

`var`变量没有时间死区。

这意味着它们在其范围内随处可见。

它的声明被提升了，但值却没有。

# `let`声明变量的生命周期

`let`变量只有在声明后才可用。

这意味着在块内，时间死区在块的开始和它们被声明之间。

这和`const`是一样的。

如果我们试图在声明这些变量之前访问它们，我们将得到一个`ReferenceError`。

如果没有给`let`变量赋值，它将是`undefined`。

例如，如果我们有:

```
let foo = true;
if (true) {
  console.log(foo); let foo;
  console.log(foo); foo = 123;
  console.log(foo);
}
console.log(foo)
```

然后`console.log(foo);`会给我们弄个`ReferenceError`。

并且:

```
let foo;
console.log(foo);
```

将日志`undefined`。

并且:

```
foo = 123;
console.log(foo);
```

日志 123。

而`foo`就是`true`。

# `typeof`为临时交易区的变量抛出一个`ReferenceError`

我们不能将`typeof`与尚未声明的`let`和`const`变量一起使用。

例如，我们可以写:

```
if (true) {
  console.log(typeof foo);
  let foo;
}
```

然后，如果我们运行代码，我们将得到一个`ReferenceError`。

如果我们想停止它，我们应该把`typeof`移到`let`声明的下面。

有一个时间死区可以让我们捕捉编程错误。

JavaScript 将来也可能有守卫来做数据类型和内容检查。

因此，在进行检查之前，我们应该确保数据可用。

如果它们只有在声明后才可用，那么检查就很容易完成。

# 循环头中的`let`和`const`

我们可以在 for、for-in 和 for-of 循环的循环头中使用`let`和`const`。

![](img/e588f237c28d150340060999478679aa.png)

丹尼尔·罗伯特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

`let`和`const`提供了很多`var`没有的好处。