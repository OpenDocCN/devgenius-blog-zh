# 现代 JavaScript 的精华——类型化数组和映射/集合问题

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-typed-arrays-and-map-set-issues-56c41d940748?source=collection_archive---------6----------------------->

![](img/51d284efe86e00e42940f7497c0eeed0.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究映射、集合和类型化数组的问题。

# 为什么地图和集合具有大小属性？

映射和集合具有`size`属性而不是`length`，因为它们不是顺序数据结构，不像数组。

`length`是指像数组这样的顺序数据结构。

# 配置贴图和集比较关键点和值的方式

没有办法配置贴图和集合如何比较它们的键和值。

这个特性很难有效且正确地实现。

# 从地图中获取东西时指定默认值

当我们从地图中获取某些东西时，没有什么比指定默认值更简单的了。

地图不让我们直接设置默认值。

但是我们可以使用`||`操作符来实现，因为当条目不存在时`get`会返回`undefined`。

因此，我们可以这样写:

```
const map = new Map();
//...
const prevCount = map.get('foo') || 0;
```

我们用键`'foo'`得到这个值。

如果是`undefined`，我们返回 0。

# 贴图与对象

地图适合保存字符串键以外的键。

否则，对象可以给我们同等的功能。

如果我们要映射任意数据，那么地图可能是更好的选择。

如果有一组固定的键，那么对象是一个不错的选择。

如果键改变了，那么地图会更好。

# 何时使用对象作为映射键？

如果我们在外部将数据附加到对象，我们可以将对象用作映射键。

但这用 WeakMaps 更好，因为如果我们删除对 key 对象的引用，就会发生垃圾收集。

# 类型化数组

类型化数组是一种特殊的数组，它允许我们保存二进制数据。

它们让我们操作图像数据、画布元素、处理二进制文件等。

此外，我们可以使用它们与 WebGL 等本地 API 进行交互，以进行图形操作。

如果没有新的数据类型，就无法操作这些 API 中的二进制数据。

两种对象与类型化数组一起工作。

它们包括缓冲区和视图。

缓冲区是`ArrayBuffer`的实例，保存二进制数据。

视图提供了访问二进制数据的方法。

有两种观点。

一种是类型化数组构造函数的实例，如`Uint8Array`、`Floar64Array`等。

这些工作起来像异常数组，但只允许一种类型的元素，没有漏洞。

另一个是`DataVBiew`的实例，它允许我们访问缓冲区中的字节偏移量。

# 处理溢出和下溢

当值超出范围时，使用模运算将其转换为范围内的值。

这意味着最高值加 1 转换为最低值。

并且最小值减 1 被转换成最大值。

类型化数组就是这种情况。

例如，如果我们有:

```
const uint8 = new Uint8Array(1);
uint8[0] = 255;
```

`uint8[0]`是 255。

另一方面，如果我们有:

```
const uint8 = new Uint8Array(1);
uint8[0] = 256;
```

`uint8[0]`为 0。

如果我们有:

```
const uint8 = new Uint8Array(1);
uint8[0] = -1;
```

`uint8[0]`是 255。

这适用于所有类型的类型化数组。

![](img/d5df19807710c2caead7fc72d4fc2b26.png)

照片由[捕捉人心。](https://unsplash.com/@dead____artist?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

地图和集合有其局限性。它们也适用于某些应用。

类型化数组用于存储二进制数据。