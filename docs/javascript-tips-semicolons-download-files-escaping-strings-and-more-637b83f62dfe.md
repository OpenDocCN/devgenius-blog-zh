# JavaScript 技巧—分号、下载文件、转义字符串等等

> 原文：<https://blog.devgenius.io/javascript-tips-semicolons-download-files-escaping-strings-and-more-637b83f62dfe?source=collection_archive---------45----------------------->

![](img/c18cb1ba2aeb286145fe5d583ce60ff0.png)

照片由 [Autri Taheri](https://unsplash.com/@ataheri?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

像任何类型的应用程序一样，当我们编写 JavaScript 应用程序时，有一些困难的问题需要解决。

在本文中，我们将研究一些常见 JavaScript 问题的解决方案。

# JavaScript 的自动分号插入(ASI)规则

如果没有分号，JavaScript 会自动插入分号。

它遵循一些插入项目的规则。

它被添加到以下代码段之后:

*   空白语句
*   `var`声明
*   做一会儿
*   `continue`
*   `break`
*   `return`
*   `throw`

因此，我们应该确保我们添加了它们或者 JavaScript 为我们做了这些。

地点可能出乎意料。

# 打破嵌套循环的最佳方式

我们可以使用`break`语句来打破嵌套循环。

例如，我们可以写:

```
for (const i of a) {
  for (const j of b) {
    breakCheck = true;
    break;
  }
  if (breakCheck) {
    break
  };
}
```

我们在内部循环中设置了`breakCheck`标志。

然后我们使用`break`来打破内部循环。

然后我们检查`breakCheck`标志并结束外部循环。

# 打印选定的元素

我们可以通过使用一些 CSS 使一个元素可打印。

如果我们有:

```
<div id=“printarea”>
  hello world
</div>
```

然后，我们可以使用下面的 CSS 使 div 可打印:

```
@media print {
  body * {
    visibility: hidden;
  }
  #printarea, #printarea * {
    visibility: visible;
  }
  #printarea {
    position: absolute;
    left: 0;
    top: 0;
  }
}
```

我们使用`print`媒体查询在打印时隐藏主体，只显示 ID 为`printarea`的 div。

# 随机颜色发生器

我们可以通过创建一个包含 6 个字符的十六进制字符串来创建一个颜色代码，从而生成一个随机颜色。

例如，我们可以写:

```
const getRandomColor = () => {
  const letters = '0123456789ABCDEF';
  const color = '#';
  for (let i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}
```

我们用`Math.floor(Math.random() * 16)`得到一个随机索引。

我们用它从`letters`数组中获得一个随机字符。

然后我们返回颜色代码串。

# 使用 Node.js 下载文件，不使用第三方库

我们可以通过创建写流来下载文件。

然后我们可以将写流传递给`http.get`方法。

然后，响应数据通过管道进入写入流。

例如，我们可以写:

```
const http = require('http');
const fs = require('fs');

const file = fs.createWriteStream("img.jpg");
const request = http.get("https://placeimg.com/640/480/any", (response) => {
  response.pipe(file);
});
```

我们使用`http`模块的`get`方法进行下载。

我们用写流调用`pipe`来保存文件。

# 自动重新加载 Node.js 中的文件

为了在代码文件改变时自动重启应用程序，我们可以使用 Nodemon。

要使用它，我们只需运行:

```
npm install nodemon -g
```

安装软件包。

然后我们运行:

```
nodemon app.js
```

使用`app.js`运行程序，其中`app.js`是我们应用程序的入口点。

# JavaScript ES6 类中的私有属性

私有类成员来 JavaScript 类。

它们由`#`字符表示。

例如，我们可以写:

```
class Foo {
  #property;

  constructor(){
    this.#property = "test";
  }

  #privateMethod() {
    return 'hello world';
  }

  getPrivateMessage() {
    return this.#privateMethod();
  }
}
```

任何前缀为`#`的都是私有成员。

它可以与巴别塔 7 一起使用，并预设第三阶段。

# 转义字符串

我们可以用反斜杠来替换所有我们想转义的字符串。

例如，我们可以写:

```
const escapeRegex = (string) => {
  return string.replace(/[-\/\\^$*+?.()|[\]{}]/g, '\\$&');
}
```

我们用一个正则表达式调用`replace`,这个正则表达式匹配我们想要转义的所有字符。

我们通过在这些字符前加一个反斜杠来代替它们。

所以如果我们称之为:

```
const escaped = escapeRegex('//');
```

那么`escaped`就是`“\/\/”`。

我们也可以使用 Lodash `escapeRegExp`方法来做同样的事情。

如果我们写:

```
const escaped = _.escapeRegExp('//');
```

![](img/0b5def134a5065a9388dd898d09ea0a0.png)

Andrew Leu 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

自动插入分号是由 JavaScript 解释器完成的。

Nodemon 允许我们观察代码文件的变化，并自动重新加载我们的应用程序。

我们可以用标志来中断嵌套循环，以结束外部循环。

我们可以创建一个写流来传输我们的响应。