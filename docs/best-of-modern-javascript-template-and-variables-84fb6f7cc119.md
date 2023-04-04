# 现代 JavaScript 的精华——模板和变量

> 原文：<https://blog.devgenius.io/best-of-modern-javascript-template-and-variables-84fb6f7cc119?source=collection_archive---------2----------------------->

![](img/a6adb73744e7fd845cb5c3fdbecd1b00.png)

照片由[蒂姆·阿特伯里](https://unsplash.com/@tim_arterbury?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

自 2015 年以来，JavaScript 有了巨大的进步。

现在用起来比以前舒服多了。

在本文中，我们将研究 JavaScript 模板文字和变量。

# 文本模板

我们为文本创建模板，因为我们可以在模板字符串中插入表达式。

例如，我们可以写:

```
const tmpl = persons => `
  <table>
  ${addrs.map(person => `
    <tr><td>${person.firstName}</td></tr>
    <tr><td>${person.lastName}</td></tr>
  `).join('')}
  </table>
`;
```

我们只是把我们的表达式传递给表达式。

# 实现标签功能

我们可以通过创建一个函数来创建自己的模板标签函数。

例如，我们可以写:

```
const tag = (...args) => {
  console.log(args);
}const firstName = 'mary';
const lastName = 'smith';tag `hello ${firstName} ${lastName}`
```

那么`parts`将是模板字符串中没有表达式的部分:

```
["hello ", " ", ""]
```

而`args`将是一个包含所有表达式的数组:

```
["mary", "smith"]
```

# 模板文字和标记模板文字从何而来？

模板文字和标记模板来自语言 E，在那里它们被称为准文字。

# 为什么使用反斜线作为模板文字的分隔符？

反斜线是 JavaScript 不使用的少数 ASCII 字符之一。

插值的`${}`对于其他语言来说是通用的。

# 变量和范围

JavaScript 给了我们两种方法来声明变量，`let`和`const`，

他们用`var`取代了 ES5 声明变量的方式。

`let`和`const`都是块范围的。

它们只存在于当前块中。

`var`是函数范围的，所以它们存在于函数中的任何地方，

例如，如果我们有:

```
if (x > y) { 
  let foo = x;
}
```

那么`foo`仅在`if`模块内可用。

这和`const`是一样的。

唯一的区别是，我们不能将一个常量的值赋给另一个值。

它们也很好，因为`let`和`const`变量不能被提升。

这意味着它们只有在被定义后才能被使用。

# 声明变量的方式

JavaScript 中有几种声明变量的方法。

`var`声明是否提升，其作用域是否确定。

它还创建全局属性。

`let`不会被提升，它是块范围的。

它也不会创建全局属性。

`const`同 let。

`function`让我们定义函数。

函数声明被完全提升。

`function`也是块范围的，创建全局属性。

关键字`class`让我们创建类。

它不是悬挂的，它是块范围的，并且不创建全局属性。

`import`让我们从其他地方导入模块成员。

它对模块是全局的，不创建全局属性。

# 通过`let`和`const`阻止作用域

使用`let`和`const`的块范围让我们创建块范围的变量。

这意味着我们可以在函数中隐藏变量。

例如，我们可以写:

```
function func() {
  const foo = 5;
  if (baz) {
    const foo = 10; 
    console.log(foo);
  }
  console.log(foo);
}
```

我们在两个地方都有`foo`。

`foo`是`if`块中的 10。

在功能块中是 5。

![](img/f3822cc65504b284138613c9b23a12a6.png)

照片由 [Gavin Allanwood](https://unsplash.com/@gavla?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用模板文字创建模板。

`let`和`const`让我们可以轻松地创建变量。