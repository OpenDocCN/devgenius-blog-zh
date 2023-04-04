# JavaScript 亮点— do…while 循环和脚本

> 原文：<https://blog.devgenius.io/highlights-of-javascript-do-while-loops-and-scripts-cdc01b50d401?source=collection_archive---------8----------------------->

![](img/96b13341e70d80449de0d13af2e40d60.png)

照片由[巴迪·阿巴斯](https://unsplash.com/@bady?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

学习 JavaScript，必须先学基础。

在本文中，我们将研究 JavaScript 语言的最基本部分。

# do…while 循环

我们可以使用`do...while`循环来重复运行代码。

`do`块总是运行的，所以第一次迭代总是运行的。

例如，我们可以写:

```
let i = 0;
do {
  console.log(i);
  i++;
} while (i <= 3);
```

`do`块总是运行的，因为`while`条件是在循环迭代完成后检查的。

# 脚本标签

要在浏览器中运行 JavaScript 代码，我们必须在代码中添加脚本标签。

例如，我们可以写:

```
<script>
  function sayHi() {
    alert("hi");
  } function sayBye() {
    alert("bye");
  }
</script>
```

我们在脚本标签中定义了`sayHi`和`sayBye`函数。

我们可以在脚本标签中包含任何 JavaScript 代码。

JavaScript 代码只是文本，所以我们可以很容易地将它们添加到 HTML 文件中，因为它们也是文本。

如果我们在脚本标签之间有很多代码，那么我们应该把它们移到它们自己的文件中。

例如，我们可以写:

```
<script src="script.js"></script>
```

我们创建一个`script.js`文件，然后用`src`属性引用它。

这样，我们可以保持代码文件简短。

# 评论

我们可以在 JavaScript 代码中添加注释来解释我们的代码在做什么。

例如，我们可以写:

```
// This is a comment
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

`//`表示注释的开始。浏览器会忽略它。

我们可以通过编写以下内容来编写多行注释:

```
/*
  Lorem ipsum dolor sit amet, consectetur adipiscing elit. Maecenas at volutpat ipsum, dapibus tincidunt nisl. Donec vestibulum ultricies laoreet. Suspendisse sit amet faucibus justo. 
*/
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

我们有一个跨越多行的长注释。由`/*`和`*/`界定。

# 链接事件

当我们点击链接时，当我们对链接做一些事情，比如点击它时，我们可以监听链接发出的事件。

除了让我们点击进入不同的页面之外，用它做其他事情也很方便。

例如，我们可以写:

```
<a href="#" onClick="alert('hello');">Click</a>
```

我们将`onClick`属性添加到`a`标签中，这样我们就可以在其中运行 JavaScript 代码。

`onClick`不区分大小写。

`href='#'`告诉浏览器重新加载页面，这很可能不是我们在点击链接时想要运行 JavaScript 代码时想要的。

为了防止页面被刷新，我们可以写:

```
<a href="javaScript:void(0)" onClick="alert('hello');">Click</a>
```

我们添加了`javaScript.void(0)`代码来避免刷新。

如果我们想写多条语句，我们可以写:

```
<a href="javaScript:void(0)" onClick="let greeting='hi'; alert(greeting);">Click</a>
```

我们用分号分隔这些语句。

# 结论

我们可以使用`do...while`循环来运行一个第一次迭代总是运行的循环。

此外，我们可以在 HTML 代码中以各种方式嵌入 JavaScript。