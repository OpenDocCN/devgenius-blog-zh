# 可维护的 JavaScript 名称

> 原文：<https://blog.devgenius.io/maintainable-javascript-names-9824bf5c29ff?source=collection_archive---------12----------------------->

![](img/89e8465d7d4b50255ec25251b5f17c22.png)

丹尼尔·赫雷斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将研究通过改进命名来创建可维护的 JavaScript 代码的基础。

# 命名

为了使我们的 JavaScript 代码可维护，我们必须使名称具有描述性，以便我们知道它们对名称做了什么。

此外，大小写应该一致，这样我们就不会与名称混淆。

变量和函数应该是骆驼大小写，

Camel case 表示名称以小写字母开头，每个后续单词以大写字母开头。

例如，这些是骆驼案例:

```
let myName;
const someVariable;
let veryLongName;
```

它们都以小写字母开头，随后的单词以大写字母开头。

# 变量和函数

变量名是骆驼大小写，应该以名词开头。

这样，我们就知道他们储存了什么。

例如，我们可以写:

```
let count = 10;
const firstName = "james";
let isTuesday = true;
```

用名词定义变量。

如果我们用其他种类的词来定义变量，比如动词，那么我们很容易把它们和函数混淆。

所以我们不应该用这样的名字来定义变量:

```
let getCount = 10;
```

暗示我们正在做一些没有意义的事情。

但是，我们应该创建以动词开头的函数:

```
function getCount() {
  return 10;
}
```

当我们调用`getCount`时，我们得到了一些值，所以这个函数从`get`开始是有意义的。

这也意味着我们的函数名不应该看起来像变量名。

例如，如果我们有:

```
function theCount() {
  return 10;
}
```

那么我们很容易把它和变量混淆。

命名与其说是一门科学，不如说是一门艺术，但是我们可以遵循一些惯例。

像`count`、`length`和`size`这样的词表示数字。

而像`name`和`message`这样的词表明它是一个字符串。

应该避免使用像`foo`或`bar`这样没有意义的名字。

函数名的一些约定包括前缀，如:

*   `can`建议返回布尔值的函数
*   `has`是一个返回布尔值的函数
*   `is`与`can`或`has`相同
*   `get`表示函数正在返回一个非布尔值
*   `set`是用于保存数值的函数

例如，如果我们有:

```
if (canSetName()) {
  setName("nick");
}
```

然后我们知道，如果我们可以设置名称，那么我们设置名称。

另一个例子是:

```
if (getName() === "nick") {
  jump();
}
```

然后我们知道我们用`getName`得到了这个名字，我们用`jump`做了一些事情。

# 常数

在 ES6 之前，没有常量的概念。

但是现在，我们可以用`const`来定义常数。

常量的名字应该是大写的。

例如，我们可以写:

```
const MAX_COUNT = 10;
```

要让大家知道`MAX_COUNT`是一个常数。

常量不能被重新赋值，但是它们是可变的。

对常数有一个单独的约定让我们能够区分变量和常数。

![](img/b64c95e64ef9c446633c0873f8ef374b.png)

照片由[桑嘎日玛罗曼塞利亚](https://unsplash.com/@sxy_selia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

对于变量、函数名和常量，我们应该遵循一些约定。