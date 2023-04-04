# 更好的 JavaScript——不应该是自动的

> 原文：<https://blog.devgenius.io/better-javascript-things-that-shouldnt-be-automatic-3ba96fc4e572?source=collection_archive---------9----------------------->

![](img/1a15a0d86b20363847b79796f0843dfd.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究改进 JavaScript 代码的方法。

# 胁迫的问题

数据强制不会将数据更改为我们期望的格式。

例如，如果我们有:

```
const date = new Date("2020/12/31");
date == "2020/12/31"
```

那么比较将是`false`。

这是因为`date`转换成了字符串 it:

```
"Thu Dec 31 2020 00:00:00 GMT-0800 (Pacific Standard Time)"
```

`toString`方法转换成它想要的格式。

要将日期转换为 YYYY/MM/DD 格式，我们可以写:

```
function toYMD(date) {
  const y = date.getFullYear(),
    m = date.getMonth() + 1,
    d = date.getDate();
    return `${y}/${(m < 10 ? "0" + m : m)}/${(d < 10 ? "0" + d : d)}`;
}
```

我们得到年、月、日。

然后我们把它们放在一个字符串中。

如果月份或日期有一个数字，那么我们在它前面加一个 0。

现在我们可以将它们作为相同格式的字符串进行比较:

```
toYMD(date) === "2020/12/31";
```

# 分号插入

JavaScript 让我们去掉分号。

然而，它只是自动为我们插入它。

这称为自动分号插入。

JavaScript 引擎自动将分号推断到我们的程序中。

分号插入机制在 ES 标准中。

这意味着该机制可以在 JavaScript 引擎之间移植。

分号插入有其缺陷。

如果我们跳过分号，我们就无法避免学习它的规则。

分号存在于`}`字符之前，一行或新行之后，或程序输入的结尾。

我们只能在一行、一个块或一个程序的末尾留下 semucolons。

所以我们可以写:

```
function Point(x, y) {
  this.x = x || 0
  this.y = y || 0
}
```

但是如果我们有:

```
function area(r) { r = +r return Math.PI * r ** 2 }
```

然后我们得到一个错误。

此外，当下一个输入标记无法解析时，会插入一个分号。

所以:

```
a = b
(foo());
```

与以下内容相同:

```
a = b(foo());
```

但是:

```
a = b
foo();
```

被解析为 2 条语句，因为:

```
a = b foo();
```

不是有效的 JavaScript 代码。

我们总是要注意下一个语句的开始，以检测我们是否可以省略分号。

如果下一行的第一个标记可以解释为语句的延续，我们就不能省略分号。

如果我们有:

```
a = b
["foo", "bar", "baz"].forEach((key) => {
  //...
});
```

那么它被解释为:

```
a = b["foo", "bar", "baz"].forEach((key) => {
  //...
});
```

括号表达式没有意义，因为我们在里面有多个项目。

如果我们有:

```
a = b
/foo/i.test(str) && bar();
```

然后这两行被解析为一条语句。

第一个`/`被视为除法运算符。

另一个问题是当我们不用分号连接文件时。

如果我们有:

`file1.js`

```
(function() {
  // ...
})()
```

`file2.js`

```
(function() {
  // ...
})()
```

那么我们可能会遇到问题。

两个文件的内容在连接时可以被视为一个语句:

```
(function() {
  // ...
})()(function() {
  // ...
})()
```

我们可以在每个文件后连接这些文件:

```
(function() {
  // ...
})();
(function() {
  // ...
})();
```

所以我们不用担心连接时的分号。

![](img/be6a17ea64367b50e743685b3efe2c43.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

省略分号有很多问题。

我们应该把它们放在我们自己身上，而不是让 JavaScript 引擎在错误的地方为我们做这些。

同样，我们应该避免自动胁迫。