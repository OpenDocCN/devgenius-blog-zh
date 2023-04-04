# 可维护的 JavaScript —多行注释

> 原文：<https://blog.devgenius.io/maintainable-javascript-multiline-comments-5221b620b36?source=collection_archive---------13----------------------->

![](img/d473ace2589db173e309cd96dfac053f.png)

照片由[杰瑞米·毕肖普](https://unsplash.com/@jeremybishop?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过创建更好的多行注释来了解创建可维护的 JavaScript 代码的基础。

# 多行注释

多行注释是可以跨越多行的注释。

它们以`/*`开始，以`*/`结束。

多行注释不必跨越多行。

例如，我们可以写:

```
/* one line comment */
```

我们也可以写:

```
/* two line
comment */
```

或者:

```
/* 
two line
comment 
*/
```

它们都是有效的，但是最好把星号放在左边，这样我们就不会和代码混淆了。

例如，我们可以写:

```
/* 
 * two line
 * comment 
 */
```

这样比较整洁，把所有评论内容都划了。

多行注释总是出现在它们描述的代码之前。

它们前面应该有一个空行，并且应该和我们正在注释的代码在同一个缩进位置。

例如，我们应该写:

```
if (hasAllPermissions) { /*
   * if you made it here,
   * then all permissions are available
   */
  allowed();
}
```

我们在注释前添加了一行，使其可读性更好。

这比:

```
if (hasAllPermissions) {
  /*
   * if you made it here,
   * then all permissions are available
   */
  allowed();
}
```

它前面没有线条。

我们还应该在星号和文本之间留一个空格。

例如，我们可以写:

```
if (hasAllPermissions) { /*
   *if you made it here,
   *then all permissions are available
   */
  allowed();
}
```

星号后面没有空格很难读。

我们还应该有适当的缩进，所以我们不应该写:

```
if (hasAllPermissions) {

/*
 *if you made it here,
 *then all permissions are available
 */
  allowed();
}
```

意图应该与注释所评论的代码相匹配。

此外，我们不应该对尾随注释使用多行注释。

例如，我们不应该写:

```
let result = something + somethingElse; /* they're always numbers */
```

我们应该把多行注释留给块注释。

# 使用注释

评论什么永远是一个有争议的话题。

但是有一些大多数人都能接受的惯例。

我们不应该做的一件事是评论代码已经描述过的事情。

所以我们不应该有这样的评论:

```
// set count
let count = 100;
```

通过查看代码，我们知道我们正在设置`count`，所以我们不需要注释。

但是如果有什么需要解释的，我们可以解释。

例如，我们可以写:

```
// this will make it rain
let count = 100;
```

现在我们知道改变`count`会下雨。

# 难以理解的代码

如果我们有难以理解的代码，那么我们应该把它注释掉。

如果仅仅阅读代码有什么难以理解的地方，那么我们应该添加一些注释。

![](img/cde03bc068f1df779ae4920ba5c39868.png)

照片由[韦斯·希克斯](https://unsplash.com/@sickhews?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

使用多行注释时，我们应该小心。

此外，如果代码中有什么不明显的地方，我们应该写一个注释。