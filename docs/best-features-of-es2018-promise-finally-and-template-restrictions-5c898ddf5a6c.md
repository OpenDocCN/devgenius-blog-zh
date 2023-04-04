# ES2018 的最佳功能—最终承诺和模板限制

> 原文：<https://blog.devgenius.io/best-features-of-es2018-promise-finally-and-template-restrictions-5c898ddf5a6c?source=collection_archive---------5----------------------->

![](img/46725d6f3bcf6f34daac73aa6eeba36c.png)

照片由[亚历山大·莱昂](https://unsplash.com/@lnalex?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将了解 ES2018 的最佳特性。

# `Promise.prototype.finally()`

将`finally`方法添加到`Promise`实例中，以运行一些代码，而不管承诺的结果如何。

例如，我们可以写:

```
promise
  .then(result => {
    //...
  })
  .catch(error => {
    //...
  })
  .finally(() => {
    //...
  });
```

当`promise`完成时，运行`then`回调。

`catch`回调在`promise`被拒绝时运行。

`finally`无论结果如何，都会进行回调。

所以:

```
promise
  .finally(() => {
    //...
  });
```

与以下内容相同:

```
promise
  .then(
    result => {
      //...
      return result;
    },
    error => {
      //...
      throw error;
    }
  );
```

最常见的用例类似于同步`finally`子句。

它让我们在使用完一个资源后进行清理。

`finally()`同`finally {}`是同步码。

`try`条款对应承诺的`then`回拨。

`catch`就像`.catch`在承诺。

`finally`就像`.finally`在承诺。

# 模板文字更改

标签函数如何与转义序列一起工作。

例如，我们可以写:

```
const raw = String.raw`\u{50}`
```

然后我们得到原始形式的`'\u{50}'`。

所以我们得到:

```
function tag(str) {
  console.log(str);
}tag `\u{50}`
```

作为`str`的值:

```
["P", raw: Array(1)]
```

熟的形式是`'P'`，生的版本是我们有的。

并不是所有的文本带有反斜杠都是非法的。

所以写下这样的话:

```
`\unicode`
```

行不通。

ES2018 取消了语法限制，因此我们可以在模板文字中输入这些类型的字符组合。

![](img/c93247e453602c571d551c4d183424b3.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

在 ES2018 中，模板文字语法现在比以前限制更少。

有些转义序列比较松散。

此外，`finally`方法被添加到`Promise`实例中，这样我们就可以运行代码，而不管承诺的结果如何。