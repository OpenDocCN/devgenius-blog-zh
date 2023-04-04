# JavaScript 最佳实践—可读性

> 原文：<https://blog.devgenius.io/javascript-best-practices-readability-b40c5c431d10?source=collection_archive---------6----------------------->

![](img/659e8c252b6e33708945d60020623868.png)

照片由[思想目录](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 不要使用逻辑运算符作为语句

我们不应该写聪明但难读的代码。

例如，类似于:

```
a() || b.foo.bar(); m && c() ? d() : l || m.baz[bee](3);
```

是不可能读懂的。

因此应该避免这种情况。

# 总是在变量作用域的顶部声明变量

我们可以将变量放在它们作用域的顶部:

```
function calculate() {
  let width, height, length;
  // ...
}
```

这样，他们很容易看到，我们可以在任何地方与他们合作。

# 重复使用的内联对象文字

如果我们有一些变化重复使用的对象文字，我们可以把它们放在一个函数中。

例如，我们可以写:

```
function prepareRequest(mode, updateId, value) {
  const req = {
    mode,
    updateId,
    value,
    uid: genUID()
  };
  //...
}
```

这样我们就可以调用`prepareRequest`并在任何地方使用它。

# 复杂的内联正则表达式

复杂的内联正则表达式是无法阅读的。

例如，如果我们有:

```
if (/^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(str)) {
  // ... 
}
```

我们不知道它在检查什么。

相反，我们可以将它赋给变量，然后像这样使用它:

```
const emailRegex = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/if (emailRegex.test(str)) {
  // ... 
}
```

现在我们知道正则表达式实际上用于检查有效的电子邮件地址。

# 处处严格平等

严格相等只是帮助我们避免自动数据类型强制的问题。

使用`==`或`!=`操作符进行数据类型转换的规则很复杂。

所以我们应该使用`===`和`!==`操作符来分别检查等式和不等式。

例如，我们可以写:

```
if (typeof x === 'string' && x === 'abc') {
  // ... 
}
```

比较它们的类型和价值。

# 假设真实等于存在

我们不应该假设真实意味着某样东西存在于一个物体中。

要检查一个键是否在一个对象中，我们应该使用`hasOwnProperty`或`in`操作符。

而不是写:

```
if (!cache[key]) {
  cache[key] = value;
}
```

我们应该写:

```
if (!cache.hasOwnProperty(key)) {
  cache[key] = value;
}
```

或者:

```
if (!(key in cache)) {
  cache[key] = value;
}
```

`in`操作符也检查原型的属性。

# 不评论或评论每一件小事

没有评论和评论一切都是不好的。

有时有些事情需要解释，但代码中没有。

然而，对代码已经涵盖的内容进行评论也是不好的。

# 不用单元测试，写一些可靠的东西

没有单元测试，很难写出可靠的东西。

当我们修改代码时，它们绝对能让我们安心。

# 使用长名字和过于具体

我们不应该有太长太具体的名字。

像这样的名字:

```
function generateNewtIDForANewRequestCacheItem() {...}
function deleteRequestCacheItemsThatDoNotEqual(value) {...}
```

它们肯定太长了，太具体了。

我们一定要想办法缩短它们的长度。

他们有很多我们不需要的额外信息。

![](img/9a3eea0f55685cec411ede33d89d5f0c.png)

照片由[杰米街](https://unsplash.com/@jamie452?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该注意一些做法，使我们的代码更容易阅读。

我们也可能在一些好的实践中走极端。