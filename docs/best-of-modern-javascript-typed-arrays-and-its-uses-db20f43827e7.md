# 现代 JavaScript 类型数组的精华及其用途

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-typed-arrays-and-its-uses-db20f43827e7?source=collection_archive---------1----------------------->

![](img/120984022079191ea416a897a9f4af25.png)

照片由[巴拉思·拉杰·N](https://unsplash.com/@bharathrajn89?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 类型化数组的用法。

# 静态类型化数组属性

类型化数组有`BYTES_PER_ELEMENT`属性来计算存储一个元素需要多少字节。

在类型化数组的原型中也有相同的属性。

# 串联类型化数组

类型化数组不像普通数组那样有一个`concat`方法。

我们可以使用`set`方法，将类型化数组作为第一个参数，偏移量索引作为第二个参数。

例如，我们可以写:

```
const arr = Int8Array.of(1, 2, 3);
const arr2 = Int8Array.of(4, 5, 6);const arrays = [arr, arr2];let totalLength = 0;
for (const arr of arrays) {
  totalLength += arr.length;
}const result = new Int8Array(totalLength);let offset = 0;
for (const arr of arrays) {
  result.set(arr, offset);
  offset += arr.length;
}console.log(result);
```

创建一个新的`result`类型化数组，长度为所有数组的总长度。

然后我们可以用`set`方法将每个类型化数组的条目添加到`result`类型化数组中。

我们在每个数组后更新`offet`来添加条目。

# 数据视图

`DataView`构造函数让我们创建一个 DataView，它的数据存储在给定的 ArrayBuffer 中。

它的签名是`(buffer, byteOffset, byteLength)`。

`buffer`是缓冲对象。

`byteOffset`是缓冲区的偏移量，以字节为单位，

`byteLength`是数据视图的长度，以字节为单位。

# 数据视图实例属性

DataView 具有以下实例属性。

`DataView.prototype.buffer`是一个 getter，它返回 DataView 的 ArrayBuffer。

`DataView.prototype.length`返回数据视图可以访问多少字节。

`DataView.prototype.byteOffset`返回 DataView 开始访问其缓冲区中的字节的偏移量。

`DataView.prototype.get`从数据视图的缓冲区返回一个值。

它将字节偏移量作为第一个参数开始访问数组。

第二个参数是是否将`littleEndian`设置为`true`。

默认是`false`。

`DataView.prototype.set`让我们将一个值写入数据视图的缓冲区。

它的签名是`(byteOffset, value, littleEndian=false)`，`byteOffset`是开始写的偏移量。

`value`美其名曰写。

`littleEndian`让我们设置数据视图的字节顺序。

# 类型化数组的用法

打字电报被用在许多地方。

文件 API 使用它来存储来自文件输入的文件数据。

例如，我们可以通过编写以下内容来获得类型化数组:

```
const fileInput = document.getElementById('fileInput');fileInput.onchange = () => {
  const file = fileInput.files[0];
  const reader = new FileReader();
  reader.readAsArrayBuffer(file);
  reader.onload = function() {
    const arrayBuffer = reader.result;    
    //...
  };
}
```

我们用`fileInput.files[0]`得到文件对象。

然后我们使用`FileReader`实例的`readAsArrayBuffer`函数将文件内容读入数组缓冲区。

`reader.result`有结果了。

# 获取 API

Fetch API 还允许我们以 ArrayBuffer 的形式获取数据。

例如，我们可以写:

```
fetch('https://file-examples-com.github.io/uploads/2017/10/file-sample_150kB.pdf')
  .then(request => request.arrayBuffer())
  .then(arrayBuffer => {
    console.log(arrayBuffer);
  });
```

从一个 URL 获取一个 PDF 文件，并在结果上使用`arrayBuffer`方法将其转换成一个 ArrayBuffer。

其他使用类型化数组的 API 包括 canvas 和 WebSockets。

![](img/559cbdb92ec2b07567b89cbe54b7fc50.png)

照片由 [Fran](https://unsplash.com/@francistogram?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

类型化数组可以像数组一样被操作。

它们被用在许多本地浏览器 API 中。