# 函数式 JavaScript — Currying

> 原文：<https://blog.devgenius.io/functional-javascript-currying-391deedc09f7?source=collection_archive---------3----------------------->

![](img/d92d8804ea7540a84b909c65d8a84968.png)

[Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

JavaScript 部分是一种函数式语言。

要学习 JavaScript，我们必须学习 JavaScript 的功能部分。

在本文中，我们将看看如何在 JavaScript 中使用 currying。

# 一元函数

一元函数是接受单个参数的函数。

例如，一元函数是:

```
const identity = (x) => x;
```

# 二元函数

二元函数是接受两个参数的函数。

其中一个例子是:

```
const add = (x, y) => x + y;
```

# 可变函数

可变函数是一个接受可变数量参数的函数。

我们可以使用 rest 操作符在 JavaScript 中定义一个变量函数:

```
function variadic(...args) {
  console.log(args)
}
```

有一个我们传入的所有参数的数组。

# Currying

Curry 正在将一个有多个参数的函数转换成嵌套的一元函数。

这很有用，因为它让我们创建应用了一些参数的函数。

例如，我们可以转换一个二元函数:

```
const add = (x, y) => x + y;
```

一元函数，通过编写以下内容返回另一个一元函数:

```
const addCurried = x => y => x + y;
```

`addCurried`函数采用参数`x`并返回一个采用参数`y`的函数，该函数返回`x`和`y`的总和。

然后我们可以通过写来使用它:

```
const add1 = addCurried(1);
const sum = add1(2);
```

我们用 1 调用了`addCurried`来返回一个将`x`设置为 1 的函数，并将它赋给`add`变量。

然后我们用 2 调用`add1`返回最终的总和。

我们可以通过创建一个函数来概括这一点，该函数返回一个带一个参数的函数，即二元函数。

在这个函数中，我们返回一个带有第二个参数的函数。

在里面，我们返回用外部函数的参数调用的二元函数的结果。

例如，我们可以写:

```
const curry = (binaryFn) => {
  return (firstArg) => {
    return (secondArg) => {
      return binaryFn(firstArg, secondArg);
    };
  };
};const add = (x, y) => x + y
const sum = curry(add)(1)(2);
```

创建`curry`函数来完成我们描述的任务。

然后我们可以像我们的`add`函数一样传入任何二元函数。

然后它会自动变成咖喱。

我们可以用一个`curry`函数来进一步推广，这个函数递归地返回带有一个参数的函数，直到只剩下一个参数。

例如，我们可以写:

```
let curry = (fn) => {
  if (typeof fn !== 'function') {
    throw Error('No function provided');
  } return function curriedFn(...args) {
    if (args.length < fn.length) {
      return function(...moreArgs) {
        return curriedFn(...[...args, ...moreArgs]);
      };
    }
    return fn(...args);
  };
};
```

我们检查`fn`是否是一个函数。

如果是，那么我们返回一个函数，该函数调用如果`fn`的参数比`args`多时返回的函数。

如果`fn`中的参数比`args`中的多，这意味着我们可以做得更多。

如果 curried 函数的参数个数与`fn`中的参数个数相同，那么我们就不能 curried 更多。

所以我们只调用函数。

那么我们可以这样称呼它:

```
const add = (x, y, z) => x + y + z;
const sum = curry(add)(1)(2)(3);
```

我们简化了函数，所以我们用它们各自的参数来调用简化的函数。

并且`sum`是 6，因为我们把所有的数字加在一起。

![](img/11e7bf531bc8386dffd052337139027e.png)

照片由 [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以修改函数，这样我们就可以创建应用了一些参数的函数，并在其他方面重用该函数。