# 可维护的 JavaScript —块

> 原文：<https://blog.devgenius.io/maintainable-javascript-blocks-af9a396a4394?source=collection_archive---------2----------------------->

![](img/68a8739918b93d711c70bfdf91006ff0.png)

照片由 [Matthijs van Schuppen](https://unsplash.com/@mattvs?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过正确编写语句来了解创建可维护的 JavaScript 代码的基础。

# 块语句

块语句应该用花括号括起来。

正文也应该在单独的一行。

跳过花括号并将所有内容放在同一行是有效的，但很难阅读。

它们也被各种风格指南和 linters 禁止，比如 ESLint 或 JSHint。

所以要加大括号，要有占据多行的块。

例如，如果我们有:

```
if (condition)
  doWork();
  doSomethingElse();
```

那么很多人会误以为两条线都在`if`块。

然而，事实并非如此。

只有第一行在`if`块内。

因此，我们应该只添加花括号，这样我们就可以清楚地分隔块。

我们写道:

```
if (condition) {
  doWork();
}
doSomethingElse();
```

让一切变得更清晰。

所有 block 语句都应使用大括号，包括:

*   如果
*   为
*   在…期间
*   做…的同时
*   试着…抓住…最后

# 支撑对齐

支架应该正确对齐。

添加大括号的一种方法是让左大括号与块的开始在同一行。

例如，我们可以写:

```
if (condition) {
  doWork();
} else {
  doSomething();
}
```

这类似于 Java 的风格，在像 Google、Airbnb、ESLint 等大多数风格指南中都有推荐。

另一种风格是将左大括号放在自己的行上。

例如，有些人可能会写:

```
if (condition) 
{
  doWork();
} else 
{
  doSomething();
}
```

这在 C#中更受欢迎，因为这是由 Visual Studio 强制执行的。

然而，没有风格指南推荐这种风格。

谷歌禁止这样做，因为它可能会引入自动分号插入错误。

# 块语句间距

块的第一行周围的间距是一个偏好问题。

有三种风格。

其中之一是在第一行的每一项之间有空格。

我们有:

```
if (condition) {
  doWork();
} else {
  doSomething();
}
```

在`if`和左括号之间，右括号和左大括号之间都有空格。

这是一些程序员的首选，因为它很紧凑，但仍有足够的空间使代码易于阅读。

另一种风格是:

```
if(condition){
  doWork();
}else{
  doSomething();
}
```

我们没有任何东西之间的空间，但它很紧凑。

所以很难推荐这种风格。

第 3rtd 样式在左括号后和右括号前添加空格，如下所示:

```
if ( condition ) {
  doWork();
} else {
  doSomething();
}
```

这种风格占用更多的空间，但可读性更强。

第一种样式是在保持可读性的同时占用的空间量之间的一个很好的折衷。

![](img/517fe51031cd25368488c9b76956c8f7.png)

尤里·卡塔拉诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 结论

块语句可以用更好的方式编写。

有很多方法可以给我们足够的空间来提高可读性，同时又不会占用太多空间。

此外，花括号应该总是包围块，以便我们总是知道他们开始或结束。