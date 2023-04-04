# ES2019 的最佳特性—捕捉、排序和 toString

> 原文：<https://blog.devgenius.io/best-features-of-es2019-catch-sort-and-tostring-2b8a0d4b8ad8?source=collection_archive---------2----------------------->

![](img/0ef9a33a769cd3a2219acfa060c0dc58.png)

照片由[杨奇煜·巴赞内格](https://unsplash.com/@fbazanegue?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2019 的最佳特性。

# 捕捉绑定

在 ES2019 中，绑定现在是可选的。

例如，我们可以写:

```
let jsonData;
try {
  jsonData = JSON.parse(str);
} catch {
  jsonData = {};
}
```

那么我们就不需要在`catch`后面的括号里得到被抛出的对象。

然而，其他与解析无关的错误将被忽略，因为我们只是将`jsonData`对象设置为一个空对象。

另一个例子是获取深度嵌套的属性。

例如，我们可以这样写:

```
function logId(item) {
  let id;
  try {
    id = item.data.id;
  } catch {
    console.log(id);
  }
}
```

为了让我们安全地获得`item`到`id`的深度嵌套的`id`属性(如果它存在的话)。

如果不存在，那么运行`catch`块中的代码。

# `Stable Array.prototype.sort()`

`Array.prototype.sort`方法现在保证是稳定的。

因此，如果在排序时某些东西被认为是相同的，那么它们将在支持 ES2019 的引擎中保持相同的顺序。

如果我们有:

```
const arr = [{
    key: 'foo',
    value: 1
  },
  {
    key: 'bar',
    value: 2
  },
  {
    key: 'foo',
    value: 3
  },
];
arr.sort((x, y) => x.key.localeCompare(y.key, 'en'));
```

那么`arr`将处于相同的顺序，不管它在哪个环境中。

# `JSON.stringify`

`JSON.stringify`现在可以单独代孕了。

所以如果我们有:

```
JSON.stringify('\u{D800}')
```

然后我们得到:

```
""\ud800""
```

# JSON 超集

JSON 字符串现在可以有`'"\u2028"'`字符。

# `Function.prototype.toString Changes`

`Function.prototype.toString`现在从 ES2019 开始有一些变化。

我们定义的函数返回一个带有原始源代码的字符串。

如果我们用内置函数调用`toString`，那么我们会看到`[native code]`。

例如，如果我们有:

```
isNaN.toString()
```

然后我们得到:

```
"function isNaN() { [native code] }"
```

用`Function`和`GeneratorFunction`引擎动态创建的函数必须创建适当的源代码并将其附加到函数上。

所有其他情况都会引发类型错误。

![](img/5c84e9c336f0d6ae4330279d16b2bbd2.png)

由[基思·约翰斯顿](https://unsplash.com/@acfb5071?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

Catch 绑定，函数`toString`，`sort`方法都和 ES2019 有变化。