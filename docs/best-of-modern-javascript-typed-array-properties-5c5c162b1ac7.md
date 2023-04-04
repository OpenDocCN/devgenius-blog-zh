# 最好的现代 JavaScript 类型化数组属性

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-typed-array-properties-5c5c162b1ac7?source=collection_archive---------5----------------------->

![](img/a7921e80fe30b1fa68504a1314ccb876.png)

克里斯·莱佩尔特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 类型化数组。

# 类型化数组

类型化数组是其元素只能是数字的数组。

可以创建纯整数类型数组的构造函数有`Int8Array`、`Uint8Array`、`Uint8ClampedArray`、`Int16Array`、`Uint16Array`、`Int32Array`和`Uint32Array`。

浮点数唯一数组包括`Float32Array`和`Float64Array`。

# 类型化数组与普通数组

类型化数组与普通数组非常相似。

它们有一个`length`属性，元素可以通过括号操作符和它们的索引来访问。

标准数组方法也是可用的。

它们的所有元素总是属于同一类型。

设置元素将值转换为允许的类型。

类型化数组总是连续的。

普通数组可以有洞，类型化数组没有。

它们总是用零初始化，因为它们不能有洞。

例如:

```
new Array(5)
```

创建一个有 5 个空插槽的数组，但是:

```
new Uint8Array(5)
```

创建具有 5 个零的类型化数组。

类型化数组也有一个关联的缓冲区。

类型化数组的元素存储在关联的 ArrayByffer 中，可以使用`buffer`属性访问该数组。

# 类型化数组是可迭代的

类型化数组是可迭代的。

它有一个`Symbol.iterator`方法使它成为一个可迭代的对象。

所以我们可以对它们使用 for-of 循环。

例如，我们可以写:

```
const arr = Uint8Array.of(1, 2, 3, 4, 5);
for (const byte of arr) {
  console.log(byte);
}
```

然后我们记录了 1、2、3、4 和 5。

# 将类型化数组与普通数组相互转换

我们可以在类型数组和普通数组之间转换。

例如，我们可以通过编写以下代码从普通数组创建类型化数组:

```
const typedArr = new Uint8Array([1, 2, 3, 4, 5]);
```

我们可以用`slice`方法或 spread 操作符将类型化数组转换成普通数组。

要调用`slice`将类型化数组转换为普通数组，我们可以写:

```
const typedArr = new Uint8Array([1, 2, 3, 4, 5]);
const arr = Array.prototype.slice.call(typedArr);
```

我们还可以通过编写以下内容来使用 spread 运算符:

```
const typedArr = new Uint8Array([1, 2, 3, 4, 5]);
const arr = [...typedArr];
```

我们也可以使用`Array.from`方法来做同样的事情。

例如，我们可以写:

```
const typedArr = new Uint8Array([1, 2, 3, 4, 5]);
const arr = Array.from(typedArr);
```

在所有 3 个例子中，我们得到`arr`的`[1, 2, 3, 4, 5]`。

# 类型数组的继承层次结构

类型化数组类都有一个公共超类。

超类不能从我们的 JavaScript 代码中直接访问。

类型化数组构造函数的`prototype`属性保存类型化数组的所有方法。

# 静态类型化数组方法

类型化数组构造函数有一些静态方法。

其中之一就是`of`法。

我们可以从中创建一个类型化数组，参数成为数组的条目。

例如，我们可以写:

```
const typedArr = Uint8Array.of(1, 2, 3, 4, 5);
```

用一些以`typedArr`结尾的号码打电话给`of`。

![](img/92837113626c8b6e36e84d8522d3f9d0.png)

照片由[钱德勒·克鲁登登](https://unsplash.com/@chanphoto?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

类型化数组类似于普通数组，但有许多使它们与众不同的属性。