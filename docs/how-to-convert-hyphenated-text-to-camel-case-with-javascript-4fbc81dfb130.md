# 如何用 JavaScript 将带连字符的文本转换成骆驼大小写？

> 原文：<https://blog.devgenius.io/how-to-convert-hyphenated-text-to-camel-case-with-javascript-4fbc81dfb130?source=collection_archive---------3----------------------->

![](img/42bff56f1ee7db53368ce3b46910920a.png)

罗伯特·梅斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们想用 JavaScript 将带连字符文本的字符串转换成骆驼大小写。

在本文中，我们将看看如何用 JavaScript 将带连字符的文本转换成骆驼大小写。

# 使用 String.prototype.replace 方法

我们可以使用 JavaScript 字符串的`replace`方法用骆驼大小写替换连字符文本。

要做到这一点，我们找到所有有连字符和后面一个字母的子字符串。

然后我们用大写字母替换它。

例如，我们可以写:

```
const str = 'my-example-setting'
const camelCased = str
  .replace(/-([a-z])/g, (g) => {
    return g[1].toUpperCase();
  });
console.log(camelCased)
```

我们有了想要转换成 camel case 的`str`字符串。

然后我们用一个正则表达式调用`replace`，这个正则表达式后面有一个连字符和一个字母。

`g`标志将搜索所有匹配正则表达式模式的实例。

`replace`的第二个参数是一个回调函数，它返回我们想要替换模式中的文本。

所以，`camelCased`就是`‘myExampleSetting’`。

# 洛达什驼峰法

另一种将带连字符的字符串转换成骆驼大小写字符串的方法是使用 Lodash `camelCase`方法。

例如，我们可以写:

```
const str = 'my-example-setting'
const camelCased = _.camelCase(str);
console.log(camelCased)
```

我们将想要转换的字符串作为`camelCase`的参数传入。

我们得到了同样的结果。

# 结论

我们可以使用 JavaScript string `replace`方法或 Lodash `camelCase`方法将带连字符的字符串转换成大小写一致的字符串。