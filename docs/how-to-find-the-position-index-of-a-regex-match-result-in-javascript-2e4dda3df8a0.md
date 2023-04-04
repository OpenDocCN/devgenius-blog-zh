# 如何在 JavaScript 中找到一个正则表达式匹配结果的位置索引？

> 原文：<https://blog.devgenius.io/how-to-find-the-position-index-of-a-regex-match-result-in-javascript-2e4dda3df8a0?source=collection_archive---------1----------------------->

![](img/d9d210c73ba9559a20057ab856d6e5ce.png)

阿米尔·拉比耶在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们希望在 JavaScript 代码中找到正则表达式匹配的索引。

在本文中，我们将看看如何用 JavaScript 找到正则表达式匹配的索引。

# 使用 RegExp.prototype.exec 方法

我们可以使用 JavaScript regex 的`exec`方法来查找 regex 匹配的索引。

例如，我们可以写:

```
const match = /bar/.exec("foobar");
console.log(match && match.index);
```

我们用`'foobar'`作为参数调用`/bar/`上的`exec`来寻找`'foobar'`字符串中的`bar`模式。

如果`match`不是`null`，那么它应该具有`match.index`属性，该属性具有`'foobar'`字符串中匹配子字符串的第一个索引。

由于`'bar'`从索引 3 开始，`match.index`应该是 3。

我们也可以用它来获取一个字符串中所有匹配的索引。

例如，我们可以写:

```
const re = /bar/g,
  str = "foobarfoobar";
while ((match = re.exec(str)) !== null) {
  console.log(match && match.index);
}
```

`re`有一个`g`标志，让我们在一个字符串中搜索一个模式的所有匹配。

我们用`while`循环遍历`re.exec`返回的结果。

如果返回的结果不是`null`，那么我们继续获取匹配的结果，直到它返回`null`。

`match.index`因此应为 3 和 9，因为第一个`'bar'`从索引 3 开始，第二个`'bar'`从索引 9 开始。

# 结论

通过使用`RegExp.prototype.exec`方法，我们可以在一个 JavaScript 字符串中找到一个或多个正则表达式模式匹配的索引。