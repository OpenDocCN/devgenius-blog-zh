# 面向对象的 JavaScript —数组

> 原文：<https://blog.devgenius.io/object-oriented-javascript-arrays-7bc71ef33526?source=collection_archive---------9----------------------->

![](img/743954dee474d53b3fc0b4fcf33eec0c.png)

照片由 [niko photos](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在这篇文章中，我们将看看数组。

# 排列

`Array`是一个内置函数，让我们创建一个数组。

例如，我们可以这样使用它:

```
const a = new Array();
```

这与以下内容相同:

```
const a = [];
```

无论哪种方式，我们都可以通过编写以下内容来填充它:

```
a[0] = 1;
a[1] = 2;
```

我们可以使用`Array`构造函数来填充数组。

例如，我们可以写:

```
const a = new Array(1, 2, 3, 4);
```

那么`a`是:

```
[1, 2, 3, 4]
```

我们可以通过传入一个整数来创建一个包含一些空槽的数组。

例如，我们可以写:

```
const a = new Array(5);
```

然后我们得到:

```
[empty × 5]
```

数组只是对象。

我们可以用`toString`方法把它转换成一个字符串。

例如，我们可以写:

```
const a = new Array(1, 2, 3, 4);
console.log(a.toString());
```

我们得到了:

```
'1,2,3,4'
```

记录在案。

我们可以用以下方法得到构造函数:

```
console.log(a.constructor);
```

我们得到了:

```
ƒ Array() { [native code] }
```

记录在案。

我们可以使用`length`属性来获取数组中的项数。

例如，我们可以写:

```
const a = new Array(1, 2, 3, 4);
console.log(a.length);
```

那么我们得到 4。

我们可以将`length`设置为不同的数字来改变数组的大小。

如果我们将`length`设置为比现在更大的值，那么我们将在数组中获得新的条目。

例如，如果我们有:

```
const a = new Array(1, 2, 3, 4);
a.length = 5;
console.log(a);
```

我们得到了:

```
[1, 2, 3, 4, empty]
```

记录在案。

如果我们将`length`设置为比现在更短的长度，那么数组将被截断。

例如，如果我们有:

```
const a = new Array(1, 2, 3, 4);
a.length = 2;
console.log(a);
```

我们得到了:

```
[1, 2]
```

# 数组方法

我们可以使用各种数组方法来操作数组。

我们可以使用`push`方法将一个条目添加到数组的末尾。

例如，如果我们有:

```
const a = [1, 2, 3, 4];
```

然后我们可以调用`push`来添加一个新条目:

```
a.push(5);
```

我们得到了:

```
[1, 2, 3, 4, 5]
```

方法删除数组的最后一个条目。

例如，我们可以写:

```
const a = [1, 2, 3, 4];
a.pop();
console.log(a);
```

然后我们得到:

```
[1, 2, 3]
```

我们可以使用`sort`方法对数组进行排序。

例如，我们可以写:

```
const a = [6, 3, 1, 3, 5];
const b = a.sort();
console.log(a);
```

然后我们得到:

```
[1, 3, 3, 5, 6]
```

它返回一个新的数组，其中的条目已经排序。

`slice`方法返回数组的一部分，而不修改源数组。

例如，如果我们有:

```
const a = [1, 2, 3, 4, 5];
const b = a.slice(1, 3);
console.log(b);
```

然后我们得到:

```
[2, 3]
```

记录在案。

`splice`方法让我们修改源数组。

我们可以用它来删除一个切片，返回它，并有选择地用新元素填充间隙。

例如，我们可以写:

```
const a = [1, 2, 3, 4, 5];
b = a.splice(1, 2, 100, 101, 102);
console.log(a);
console.log(b);
```

那么`a`就是:

```
[1, 100, 101, 102, 4, 5]
```

而`b`是:

```
[2, 3]
```

我们用 2 个条目替换了索引 1 中的值。

然后我们用 100，101 和 102 来代替它。

移除的条目将被返回。

![](img/0ea6c52e6bb15bc2572609580fef2c59.png)

亚历克斯·霍利奥克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

数组让我们按顺序存储数据。我们可以用各种方法来操纵它们。