# 现代 JavaScript 的精华——数组漏洞和操作

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-array-holes-and-operations-de6b0407f93b?source=collection_archive---------7----------------------->

![](img/7611fe125d6c609c11b38c76b5782fcf.png)

[Kittikom Chaichum](https://unsplash.com/@palmografia?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究一些有洞的数组操作。

# 数组操作和孔

各种操作以不同的方式处理数组孔。

迭代是通过调用`Array.prototype[Symbol.iterator]`完成的。

它将孔视为条目`undefined`。

如果我们直接给`Symbol.iterator`打电话，我们就能看到这一点。

例如，我们可以写:

```
const arr = [, 'foo'];
const iter = arr[Symbol.iterator]();console.log(iter.next())
console.log(iter.next())
```

我们从返回迭代器，让我们用迭代器顺序获取数组的元素。

如果我们第一次调用`next`，我们得到:

```
{ value: undefined, done: false }
```

如果我们再次调用`next`，我们得到:

```
{ value: "foo", done: false }
```

正如我们所看到的，第一个`next`调用返回的是`value`即`undefined`。

spread 操作符也以同样的方式处理孔。

例如，我们可以写:

```
const arr = [, 'foo'];
console.log([...arr]);
```

在我们展开`arr`数组后，我们可以看到记录的值是:

```
[undefined, "foo"]
```

for-of 循环也以同样的方式处理孔。

例如，如果我们写:

```
const arr = [, 'foo'];for (const x of arr) {
  console.log(x);
}
```

然后我们得到`undefined`和`'foo'`。

`Array.from()`使用迭代将可迭代对象转换为数组。

这与 spread 运算符完全一样。

例如，如果我们有:

```
const arr = [, 'foo'];console.log(Array.from(arr));
```

然后我们得到:

```
[undefined, "foo"]
```

如果我们将一个不可迭代的类似数组的对象传入`Array.from`方法，丢失的条目仍然被视为`undefined`。

如果我们有:

```
const arr = Array.from({
  1: 'foo',
  length: 2
})
```

然后我们得到同样的结果。

对于第二个参数，`Array.from`的工作方式类似于`Array.prototype.map`。

例如，如果我们写:

```
const arr = Array.from([, 'foo'], x => x)console.log(arr);
```

那么`arr`就是`[undefined, “foo”]`。

我们也可以用它来得到指数。为此，我们写道:

```
const arr = Array.from([, 'foo'], (x, i) => i)
```

那么`arr`就是`[0, 1]`。

跳过洞，但保留它们。

例如，如果我们有:

```
const arr = [, 'foo'].map(x => x)console.log(arr);
```

然后我们得到:

```
[empty, "foo"]
```

贴图后，该孔将保留为空槽。

如果我们将一个有孔的数组映射到一个索引数组，情况也是如此。

如果我们写:

```
const arr = [, 'foo'].map((x, i) => i)
```

我们得到`[empty, 1]`。

不同的数组实例方法对数组孔的行为有不同的处理方式。

`forEacg`、`filter`、`some`忽略孔洞。

`map`跳过但保留漏洞。

`join`和`toString`像对待`undefined`一样对待球洞，

`null`和`undefined`被视为空字符串。

`copyWithin`复制孔时创建一个孔。

`entries`、`keys`、`values`、`find`和`findIndex`将每个孔视为`undefined`。

`fill`不在乎有没有洞。

`concat`、`map`、`push`、`reverse`、`slice`、`sort`、`splice`、`unshift`均为预留孔。

`pop`和`shift`把它们当做元素。

![](img/e463f62d3ef16a46fc8f821b35a64255.png)

照片由[安德烈亚斯·菲克尔](https://unsplash.com/@afafa?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

不同的运算符和方法会以不同的方式处理数组孔。