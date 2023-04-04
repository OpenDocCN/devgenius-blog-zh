# JSON 相当于 XQuery/XPath 吗？

> 原文：<https://blog.devgenius.io/what-is-json-equivalent-of-xquery-xpath-739ed353c61f?source=collection_archive---------3----------------------->

![](img/0d113694d5afd80707597decdb2207a0.png)

照片由[在](https://unsplash.com/@ugnehenriko?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果我们操作 XML，我们可以使用 XQuery 或 XPath 查询语言在 XML 代码中找到我们想要的元素。

在 JSON 中，在任何运行时环境中都没有这种内置的查询语言，但是有许多库可以让我们做同样的事情。

在本文中，我们将研究 XQuery 或 XPath 与 JSON 的等效物。

# JSPath

我们可以用来查询 JSON 对象的一个库是 JSPath 库。

要使用它，我们可以运行:

```
npm i jspath
```

来安装它。

然后我们可以用它来查询 JSON，方法是:

```
import JSPath from "jspath";const results = JSPath.apply(
  '.automobiles{.maker === "Honda" && .year > 2009}.model',
  {
    automobiles: [
      { maker: "Nissan", model: "Teana", year: 2000 },
      { maker: "Honda", model: "Jazz", year: 2023 },
      { maker: "Honda", model: "Civic", year: 2007 },
      { maker: "Toyota", model: "Yaris", year: 2008 },
      { maker: "Honda", model: "Accord", year: 2011 }
    ],
    motorcycles: [{ maker: "Honda", model: "ST1300", year: 2012 }]
  }
);console.log(results);
```

我们调用`JSPath.apply`来查询第二个参数中的对象。

第一个参数是查询的字符串。

我们在`automobiles`数组中搜索`maker`等于`'Honda'`并且`year`大于 2009 的项目。

我们得到每个对象的`model`属性。

因此，`result`就是:

```
["Jazz", "Accord"]
```

它还支持带有`*`的通配符查询。

例如，我们可以写:

```
import JSPath from "jspath";const results = JSPath.apply(
  '.automobiles{.maker === "Honda" && .year > 2009}.*',
  {
    automobiles: [
      { maker: "Nissan", model: "Teana", year: 2000 },
      { maker: "Honda", model: "Jazz", year: 2023 },
      { maker: "Honda", model: "Civic", year: 2007 },
      { maker: "Toyota", model: "Yaris", year: 2008 },
      { maker: "Honda", model: "Accord", year: 2011 }
    ],
    motorcycles: [{ maker: "Honda", model: "ST1300", year: 2012 }]
  }
);console.log(results);
```

那么`result`就是:

```
["Honda", "Jazz", 2023, "Honda", "Accord", 2011]
```

我们用`*`操作符获得匹配结果对象的所有属性值。

# JSONPath

我们可以用来查询 JSON 对象的另一个库是使用 JSONPath 库。

要安装它，我们运行:

```
npm i jsonpath
```

然后我们可以通过写来使用它:

```
const jp = require("jsonpath");
const persons = [
  { name: "James", age: 36 },
  { name: "Mary", age: 34 },
  { name: "Jane", age: 35 },
  { name: "Bob", age: 28 }
];const names = jp.query(persons, "$..name");
console.log(names);
```

我们调用`jp.query`用 `“$..name”`查询从`persons`数组的每个条目中获取`name`属性。

`$`表示根对象。

`..`意味着我们递归地寻找属性。

因此，我们有:

```
["James", "Mary", "Jane", "Bob"]
```

由于`names`。

我们还可以通过编写以下内容来查询满足给定条件的对象:

```
const jp = require("jsonpath");
const obj = {
  persons: [
    { name: "James", age: 36 },
    { name: "Mary", age: 34 },
    { name: "Jane", age: 35 },
    { name: "Bob", age: 28 }
  ]
};const names = jp.query(obj, "$..persons[?(@.age>30)]");
console.log(names);
```

我们调用`jp.query`来查找`persons`数组属性中`age`大于 30 的对象。

所以我们得到:

```
[
  {
    "name": "James",
    "age": 36
  },
  {
    "name": "Mary",
    "age": 34
  },
  {
    "name": "Jane",
    "age": 35
  }
]
```

作为`names`的值。

# 结论

我们可以使用 JSPath 和 JSONPath 库来查询 JavaScript 对象中的项目。