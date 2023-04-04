# ES2018 的最佳功能—对象静止和扩散

> 原文：<https://blog.devgenius.io/best-features-of-es2018-object-rest-and-spread-161798d60afa?source=collection_archive---------11----------------------->

![](img/9ea265de3add7efe6fb1a5d8043b0aba.png)

安吉丽娜·基丘科娃在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2018 的最佳特性。

# 对象上的 Rest 运算符

我们可以在对象中使用 rest 操作符进行析构。

例如，我们可以写:

```
const obj = {
  a: 1,
  b: 2,
  c: 3
};
const {
  a,
  ...rest
} = obj;
```

那么`a`是 1 而`rest`是:

```
{
  "b": 2,
  "c": 3
}
```

如果我们使用对象析构来处理命名参数，rest 操作符让我们收集所有剩余的参数。

所以如果我们写:

```
function foo({
  a,
  b,
  ...rest
}) {
  console.log(
    a,
    b,
    rest
  );
  return a + b;
}foo({
  a: 1,
  b: 2,
  c: 3
})
```

那么`a`就是 1，`b`就是 2，`rest`就是`{c: 3}`。

我们最多可以使用 rest 操作符进行一次析构，并且只能在最后使用。

这是有意义的，因为这是 JavaScript 引擎知道什么还没有通过析构赋给变量的唯一方式。

所以我们不能写:

```
const {...rest, a} = obj;
```

或者:

```
const {foo, ...a, ...b} = obj;
```

它们都会给我们带来语法错误。

我们可以嵌套 rest 操作符。

例如，我们可以写:

```
const obj = {
  qux: {
    a: 1,
    b: 2,
    c: 3,
  },
  foo: 4,
  baz: 5,
  bar: 6,
};
const {
  qux: {
    a,
    ...rest1
  },
  ...rest2
} = obj;
```

那么`a`就是 1，`rest1`就是`{b: 2, c: 3}`。

而`rest2`就是`{foo: 4, baz: 5, bar: 6}`。

# 对象文字中的扩展运算符

对象文字中的扩展运算符是 ES2018 的新增功能。

例如，我们可以这样使用它:

```
const obj = {
  a: 1,
  b: 2
};const obj2 = {
  ...obj,
  baz: 3
}
```

我们使用 spread 操作符将`obj`的属性复制到一个新对象中，并将其分配给`obj2`。

所以`obj2`就是`{a: 1, b: 2, baz: 3}`。

顺序很重要，因为它们将按照传播的顺序进行填充。

所以如果我们有:

```
const obj = {
  a: 1,
  b: 2
};const obj2 = {
  baz: 3,
  ...obj
}
```

那么`obj2`就是`{baz: 3, a: 1, b: 2}`。

这两个键冲突，后一个键会覆盖前一个键。

所以如果我们有:

```
const obj = {
  a: 1,
  baz: 2
};const obj2 = {
  baz: 3,
  ...obj
}
```

然后我们得到:

```
{baz: 2, a: 1}
```

作为`obj2`的值。

# 对象扩展运算符的使用

对象扩展操作符有几个用例。

其中之一就是克隆一个物体。

例如，我们可以写:

```
const obj = {
  a: 1,
  b: 2
};const clone = {
  ...obj
};
```

然后用 spread 运算符对`obj`进行浅层复制，因此`clone`为:

```
{
  a: 1,
  b: 2
}
```

这与使用`Object.assign`进行克隆是一样的；

```
const obj = {
  a: 1,
  b: 2
};const clone = Object.assign({}, obj);
```

传播更短，更干净。

![](img/2cce783df388bdd4588447cd77251225.png)

由[德鲁·科夫曼](https://unsplash.com/@drewcoffman?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

“对象静止”和“展开”操作符是 ES2018 中发布的全新操作符。

既干净又方便。