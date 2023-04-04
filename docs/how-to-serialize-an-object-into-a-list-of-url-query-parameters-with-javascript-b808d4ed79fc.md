# 如何用 JavaScript 将一个对象序列化成 URL 查询参数列表？

> 原文：<https://blog.devgenius.io/how-to-serialize-an-object-into-a-list-of-url-query-parameters-with-javascript-b808d4ed79fc?source=collection_archive---------2----------------------->

![](img/f6d081d43468d5f3523ed1e233de6f2b.png)

[安德鲁·尼尔](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

有时，我们希望将 JavaScript 对象的键值对转换成查询字符串，并将对象中的键值对作为查询参数。

在本文中，我们将研究如何用 JavaScript 将一个对象序列化为一组 URL 查询参数。

# 使用 URLSearchParams 构造函数

我们可以使用大多数现代浏览器都有的`URLSearchParams`构造函数将 JavaScript 对象转换成查询字符串。

例如，我们可以写:

```
const obj = {
  param1: 'something',
  param2: 'somethingelse',
  param3: 'another'
}
const url = new URL(`http://example.com`);
url.search = new URLSearchParams(obj);
console.log(url.toString())
```

创建我们想要转换成查询字符串的`obj`对象，并将键-值对添加到查询字符串中。

然后我们用`URL`创建`url`对象，以创建`URL`实例。

接下来，我们将`url.search`属性设置为我们想要设置的查询字符串。

为了创建查询字符串，我们使用带有`obj`的`URLSearchParams`构造函数来创建查询字符串对象。当我们将查询字符串转换为字符串时，它将被追加到 URL 中。

在最后一行，我们调用`toString`将`URL`实例转换成一个 URL 字符串。

因此，控制台日志应该记录:

```
http://example.com/?param1=something&param2=somethingelse&param3=another
```

# 结论

我们可以用`URLSearchParams`构造函数将 JavaScript 对象序列化为查询字符串。