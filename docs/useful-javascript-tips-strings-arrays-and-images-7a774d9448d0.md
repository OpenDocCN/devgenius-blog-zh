# 有用的 JavaScript 技巧——字符串、数组和图像

> 原文：<https://blog.devgenius.io/useful-javascript-tips-strings-arrays-and-images-7a774d9448d0?source=collection_archive---------31----------------------->

![](img/d330f791674e83295954b4ae3532b16f.png)

彼得·温特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 把一串剪成单词

要将一个字符串分割成单词，我们可以使用`split`方法。

例如，我们可以写:

```
const text = "hello james smith";
const arr = text.split(" ");
```

那么`text`将会按照它们的空格被分割成一个字符串数组。

所以`arr`就是`[“hello”, “james”, “smith”]`。

# 将图像载入画布

我们可以通过使用画布上下文的`drawImage`方法将图像加载到画布中。

例如，如果我们有以下画布:

```
<canvas width="300" height="300"></canvas>
```

和图像元素:

```
<img id='cat' style='visibility: hidden' src='http://placekitten.com/200/200' />
```

我们可以写:

```
const canvas = document.querySelector('canvas');
const context = canvas.getContext('2d');
const img = document.getElementById("cat");
context.drawImage(img, 90, 90, 50, 60, 10, 10, 50, 60);
```

`drawImage`的第一个参数是图像元素。

第二个参数是图像左上角的 x 坐标。

3 号是左上角的 y 坐标。

第四个是源图像的子矩形的宽度。

第五个是源图像的子矩形的高度。

第 6 个是放置源左上角的目标画布的 x 坐标。

第 7 个是左上角的 y 坐标。

第 8 个是图像到目标画布的宽度。

最后一个是图像在目的地的高度。

# 减慢循环速度

我们可以用承诺来减缓循环。

首先，我们用`setTimeout`函数创建自己的`sleep`函数:

```
const sleep = (ms) => {
  return new Promise(resolve => setTimeout(resolve, ms))
}
```

我们返回一个运行`setTimeout`并调用`resolve`的承诺。`ms`是以毫秒为单位的延迟。

然后我们可以通过运行以下命令循环运行`sleep`函数:

```
const list = [1, 2, 3];
(async () => {
  for (const item of list) {
    await sleep(1000);
    console.log(item)
  }
})();
```

我们有一个数组`list`，在一个异步函数中循环。

在循环体中，我们用 1000 调用`sleep`来延迟循环 1 分钟。

然后我们用`console.log`记录该项目。

# 获取数组中的前 n 项

要获得数组中的第一个`n`项，我们可以使用`slice`方法。

例如，我们可以写:

```
const arr = [1, 2, 3, 4, 5];
const newArray = arr.slice(0, 2);
```

在新数组中返回来自`arr`的前 2 个数组条目。

所以`newArray`就是`[0, 1]`。

# 将数组转换为字符串

我们可以调用`toString`方法将数组转换成字符串。

例如，我们可以写:

```
const list = [1, 2, 3];
const str = list.toString();
```

然后我们得到`“1,2,3”`作为`str`的值。

它不是很灵活，因为它转换它喜欢的方式。

为了增加灵活性，我们可以使用`join`方法。

例如，我们可以写:

```
const list = [1, 2, 3];
const str = list.join();
```

将数组的条目连接成一个字符串。

我们可以传入一个分隔符来分隔条目。

所以我们可以写:

```
const list = [1, 2, 3];
const str = list.join(', ');
```

使用逗号作为分隔符。

# 展平数组

我们可以用`flat`和`flatMap`来展平数组。

例如，我们可以写:

```
const animals = ['dog', ['cat', 'wolf']];
const flattened = animals.flat();
```

我们调用了`flat`，这是一个内置的数组方法。

然后我们看到`flattened`就是`[“dog”, “cat”, “wolf”]`。

`flat`可以展平任意级别的数组。

如果我们传入 2，那么它将平铺 2 个嵌套层次。

如果我们传入`Infinity`，那么展平是递归完成的。

所以如果我们打电话给:

```
const animals = ['dog', [['cat', 'wolf']]];
const flattened = animals.flat(2);
```

我们得到同样的结果。

如果我们写:

```
const animals = ['dog', [['cat', 'wolf']]];
const flattened = animals.flat(Infinity);
```

我们得到了完全的展平，这也意味着同样的结果。

![](img/5b44c877544ed2ae17d84bbee65ff32b.png)

[弗吉尼亚·蔡](https://unsplash.com/@veechoy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

我们可以用`flat`轻松地展平数组。

此外，我们可以用`drawImage`将图像从 img 标签加载到画布中。

为了暂停 JavaScript 代码的执行，我们可以使用带有承诺的`setTimeout`。