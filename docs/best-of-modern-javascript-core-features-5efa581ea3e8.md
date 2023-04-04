# 现代 JavaScript 的精华—核心特性

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-core-features-5efa581ea3e8?source=collection_archive---------6----------------------->

![](img/0f60af5ccef48d138a8df9565eb8446a.png)

照片由 [dimitris pantos](https://unsplash.com/@dpantos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 的核心特性。

# 从变量到常量/let

`var`不再是唯一用来声明变量的关键字。

有更好的选择是`let`和`const`。

`var`是函数作用域，可以提升。

这使得确定用`var`声明变量的位置变得困难。

另一方面，确定用`let`或`const`声明的变量很容易。

它们都是块范围的，所以我们知道它们只在块内可用。,

例如，如果我们有:

```
var x = 100;function foo(randomize) {
  if (randomize) {
    var x = Math.random();
    return x;
  }
  return x;
}
foo(false);
```

如果不看每行，我们就不知道`x`会是什么。

另一方面，如果我们有:

```
let x = 100;function foo(randomize) {
  if (randomize) {
    let x = Math.random();
    return x;
  }
  return x;
}
foo(false);
```

然后我们知道外面的`x`和里面的`x`没有任何关系。

`x`外功能为 100。

而函数里面的`x`就是随机数。

所以我们要用`let`或者`const`来声明变量。

如果我们不想在变量声明后改变它的值，那么我们用`const`声明它。

我们不能盲目的用`let`或`const`代替`var`。

我们要确保替换后逻辑仍然正确。

但是从现在开始，我们大部分时间用`const`，对于可以变化的变量用`let`，从来不用`var`。

# 生活在街区

为了在 ES6 之前创建私有变量，我们将它们放在 IIFEs 中。

IIFEs 代表立即调用的函数表达式。

这是一个看起来像:

```
(function() { 
  var foo = '...'
}());
```

我们有一个函数，用圆括号括起来，叫做。

`tmp`功能外不可用。

使用 ES6，我们可以使用带有`let`或`const`关键字的块来创建私有变量:

```
{
  let foo = '..';
}
```

我们创建了变量`foo`，它只在程序块内部可用。

# 将字符串连接到模板文字

有了 ES6，我们终于有了模板文字。

他们让我们在字符串中插入表达式。

这比串联好得多，因为它们更容易准备和调试。

例如，不写:

```
function getDimensions(width, height) {
  console.log('width: ' + width + ', height: ' + height);
}
```

我们写道:

```
function getDimensions(width, height) {
  console.log(`width: ${width},  height: ${height}`);
}
```

# 多行字符串

我们可以很容易地用模板文字生成多行字符串。

我们只需要按照我们想要的方式把它们放到绳子上。

例如，不写:

```
let html = '<!doctype html>\n' +
  '<html>\n' +
  '<head>\n' +
  '    <meta charset="UTF-8">\n' +
  '    <title></title>\n' +
  '</head>\n' +
  '<body>\n' +
  '</body>\n' +
  '</html>\n
```

我们写道:

```
let html = `
<!doctype html>
<html> <head>
    <meta charset="UTF-8">
    <title></title>
  </head> <body>
  </body></html>
`
```

它要干净得多，我们不必再做任何连接。

![](img/caf39f860f29eed54dd71037e91eacbd.png)

由[维克多·塔拉舒克](https://unsplash.com/@viktortalashuk?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

现代 JavaScript 的核心特性是`let`、`const`和模板字符串。