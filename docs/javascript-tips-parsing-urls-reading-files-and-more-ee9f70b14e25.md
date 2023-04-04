# JavaScript 技巧—解析 URL、读取文件等等

> 原文：<https://blog.devgenius.io/javascript-tips-parsing-urls-reading-files-and-more-ee9f70b14e25?source=collection_archive---------28----------------------->

![](img/500c1e1e18cf933ca4f91101af98652e.png)

照片由[德韦恩·希尔斯](https://unsplash.com/@dhillssr?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# 将 URL 解析为主机名和路径

为了将 URL 字符串解析成它的主机名和路径，我们可以使用`URL`构造函数。

例如，我们可以写:

```
new URL("http://example.com/foo/bar")
```

然后返回一个具有`hostname`和`pathname`属性的对象。

`hostname`是主机名。

`pathname`是路径名。

# 将 JavaScript 日期格式化为 yyyy-mm-dd

我们可以用一些日期方法将日期格式化为 yyyy-mm-dd 格式的字符串。

例如，我们可以写:

```
const d = new Date(date);
let month = (d.getMonth() + 1).toString();
let day = (d.getDate()).toString();
const year = d.getFullYear();if (month.length < 2) {
  month = `0${month}`;
}if (day.length < 2)  {
  day = `0${day}`;
}const formattedDate = [year, month, day].join('-');
```

我们从`Date`构造函数中创建一个日期对象。

然后我们调用`getMonth`来获取月份。

我们必须加 1，因为月份是从零开始的。

我们用`getDate`得到一个月中的某一天。

同样，我们用`getFullTear`得到 4 位数的年份。

如果月或日是一位数，那么我们在它前面加一个零。

然后我们用`'-'`把年、月、日连在一起。

一个更简单的解决方案是使用`toISOString`方法。

例如，我们可以写:

```
const formattedDate = new Date().toISOString().slice(0, 10);
```

我们将日期转换为字符串，并提取年、月和日部分。

# 检查元素在 DOM 中是否可见

我们可以用几种方法检查一个元素在 DOM 中是否可见。

一种方法是使用`offsetParent`属性来检查可见性。

每当通过 display style 属性隐藏该属性或其父属性时，该属性将返回`null`。

如果页面上没有固定元素，就会出现这种情况。

如果页面上没有固定的元素，那么我们写:

```
el.offsetParent === null
```

其中`el`是要检查可见性的元素，用于检查可见性。

我们也可以用一个元素作为它的参数来调用`getComputedStyle`以获得它计算的`display`样式。

所以我们可以写:

```
cpnst style = window.getComputedStyle(el);
const isInvisibie = style.display === 'none';
```

我们检查`style.display`属性是否为`'none'`。

# 将字符串转换为字符数组

我们可以用`split`方法将一个字符串分割成一个字符数组。

例如，我们可以写:

```
const output = "foo bar".split('');
```

然后我们得到:

```
["f", "o", "o", " ", "b", "a", "r"]
```

作为`output`的值。

# 获取对象键的数组

要获得一个对象键的数组，我们可以使用`Object.keys`方法。

例如，我们可以写:

```
const obj = {
  foo: 1,
  bar: 2
};const keys = Object.keys(obj);
```

那么我们`keys`就是`['foo', 'bar']`。

# 用{}或新对象()创建一个空对象

使用`new Object()`没有任何好处。

并且使用`{}`更加简洁易读。

因此，我们应该使用 object literal 语法而不是`Object`构造函数。

# 读取本地文本文件

我们可以用客户端 JavaScript 读取本地文本文件。

为此，我们可以使用`FileReader`构造函数来完成。

我们可以让用户选择一个文件并阅读所选的文本文件。

例如，我们可以添加一个文件输入:

```
<input type='file' accept='text/plain' onchange='openFile(event)'>
```

然后我们可以写:

```
const openFile = (event) => {
  const input = event.target;
  const reader = new FileReader();
  reader.onload = () => {
    const text = reader.result;
    consr node = document.getElementById('output');
    node.innerText = text;
    console.log(reader.result);
  };
  reader.readAsText(input.files[0]);
};
```

我们创建了接受一个`event`参数的`openFile`函数。

我们用`event.target`得到输入元素。

然后我们用`files[0]`得到选中的文件。

接下来，我们创建一个`FileReader`实例，并使用文件作为参数对其调用`readAsText`。

然后`onload`回调将运行，我们用`reader.result`得到结果。

![](img/b10c6a0f7adde4c1767c2cc3f4983651.png)

照片由[达里奥·巴伦苏埃拉](https://unsplash.com/@darvalife?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以使用`URL`构造函数将 URL 解析成各个部分。

`FileReader`让我们读取文本文件。

要在没有库的情况下格式化日期，我们可以使用`toISOString`或方法来获取日期部分。

有几种方法可以检查元素是否可见。