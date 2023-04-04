# 如何从 JavaScript 数组中获取子数组？

> 原文：<https://blog.devgenius.io/how-to-get-a-subarray-from-a-javascript-array-ecdb73d0f0e6?source=collection_archive---------5----------------------->

![](img/6e4b90f221f2e65bea33dbcd27faa18b.png)

Pierre Bamin 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

有时，我们可能想从 JavaScript 数组中获取一个子数组。

在本文中，我们将研究如何从 JavaScript 数组中获取子数组。

# 数组.原型.切片

JavaScript 数组的`slice`实例方法获取我们想要提取的数组的开始和结束索引。

不包括结束索引。

它返回一个数组，数组中的项目从起始索引开始，到结束索引减 1。

例如，我们可以写:

```
const arr = [1, 2, 3, 4, 5];
const sub = arr.slice(1, 3)
console.log(sub)
```

那么`sub`就是`[2, 3]`，因为我们从索引 1 到 2 中提取数组并返回它。

我们还可以将负索引传递给`slice`方法。

负索引意味着我们从数组的末尾开始计数，而不是从开始计数。

所以-1 是数组的最后一个元素，-2 是倒数第二个元素，依此类推。

因此，我们可以将上面的代码替换为:

```
const arr = [1, 2, 3, 4, 5];
const sub = arr.slice(1, -2)
console.log(sub)
```

并为`sub`得到相同的值。

# 结论

我们可以使用 JavaScript 数组的`slice`方法从 JavaScript 数组中提取一个子数组。