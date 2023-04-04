# JavaScript 技巧—转换字节大小、删除空白属性等等

> 原文：<https://blog.devgenius.io/javascript-tips-converting-byte-sizes-removing-blank-attributes-and-more-83c9ffd1c64d?source=collection_archive---------34----------------------->

![](img/f886311c84d6e3b381b4f747d9995a45.png)

谢尔盖·阿库利奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# DOMContentLoaded 和 Load 事件之间的区别

DOMContentLoaded 和 load 事件是有区别的。

DOMContentLoaded 事件在文档加载和解析时被触发，无需等待样式表、图像和子框架等资源完成加载。

load 事件等待所有内容完成加载。

# 变量前的加号

`+`操作符将非数字变量转换成数字。

例如，如果我们有:

```
const str = '123';
const num = +str;
```

那么`num`就是 123。

# 确定字符串是否在 JavaScript 的数组中

我们可以使用`indexOf`方法来检查一个字符串是否在数组中。

例如，我们可以写:

```
['foo', 'bar', 'baz'].indexOf(str) > -1
```

我们可以调用`indexOf`来检查`str`是否包含在`['foo', 'bar', 'baz']`数组中。

如果不在数组中，返回-1，如果在数组中，返回索引。

同样，我们可以使用`includes`方法来做同样的事情:

```
['foo', 'bar', 'baz'].includes(str)
```

如果包含，则返回`true`，如果不包含，则返回`false`。

# 将字节大小转换为 KB、MB、GB 等。在 JavaScript 中

我们把字节的大小转换成 KB，MB 等。用字节数的 1024 次方除以我们要转换的单位的次方。

例如，我们可以写:

```
const formatBytes = (bytes, decimals) => {
  if (bytes == 0) {
    return "0 Byte";
  }
  const k = 1024;
  const sizes = ["Bytes", "KB", "MB", "GB", "TB", "PB"];
  const i = Math.floor(Math.log(bytes) / Math.log(k));
  return `${parseFloat((bytes / Math.pow(k, i)).toFixed(decimals))} ${sizes[i]}`;
}
```

我们返回的`'0 bytes'`是`bytes`是 0。

要将`bytes`转换为给定的单位，我们可以将它除以 1024 的幂。

`i`是我们想要得到的`sizes`数组的索引，也是我们想要 1024 的指数。

如果我们除以`1024`，我们得到以 KB 为单位的大小。

如果我们除以`1024 ** 2`，我们会得到以 MB 为单位的大小，以此类推。

现在我们得到了尺寸和相应的单位。

# 将 JS 文件包含在另一个 JS 文件中

我们可以通过动态创建脚本标签将一个 JS 文件包含在另一个 JS 文件中。

例如，我们可以写:

```
const script = document.createElement('script');
script.src = '/path/to/script';
document.head.appendChild(script);
```

创建脚本标签。

然后我们将它附加到`head`元素来加载脚本。

# 在 Javascript 中移除对象的空白属性

在 JavaScript 中，我们可以通过使用`Object.keys`方法获取键来删除对象的空白属性。

然后我们可以使用`delete`删除值为空的键。

例如，我们可以写:

```
for (const key of Object.keys(obj)) {
  if (obj[key] === null || typeof obj[key] === 'undefined') {
    delete obj[key];
  }
}
```

我们用 for-of 循环遍历这些键。

在循环体中，我们检查`null`和`undefined`。

然后我们使用`delete`删除值为`null`或`undefined`的属性。

# 按带有日期值的单键对对象数组进行排序

如果我们有一个属性是日期字符串的对象数组，我们可以使用`sort`方法按照日期字符串属性对数组进行排序。

例如，如果我们有以下数组:

```
const arr = [{
    updatedAt: "2012-01-01T06:25:24Z",
    foo: "bar"
  },
  {
    updatedAt: "2012-01-09T11:25:13Z",
    foo: "bar"
  },
  {
    updatedAt: "2012-01-05T04:13:24Z",
    foo: "bar"
  }
]
```

然后我们可以写:

```
const sorted = arr.sort((a, b) => {
  const updatedAtA = new Date(a.updatedAt);
  const updatedAtB = new Date(b.updatedAt);
  return updatedAtA - updatedAtB;
});
```

我们有一个回调，减去`updatedAtA`和`updatedAtB`。

这是可行的，因为在它们被减去之前，它们首先被转换成它们的时间戳值。

因此，我们可以进行排序，因为将返回一个数字。

![](img/c5913adc28f1432bf479c2dfb302cf27.png)

照片由 [AZGAN MjESHTRI](https://unsplash.com/@azganmjeshtri?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

DOMContentLoaded 和 load 是不同的事件。

我们可以直接对日期进行排序。

`+`运算符可用于转换成数字。

我们可以通过做一些除法把字节转换成不同的单位。