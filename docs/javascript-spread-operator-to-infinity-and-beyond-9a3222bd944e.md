# JavaScript spread 运算符:无限远！

> 原文：<https://blog.devgenius.io/javascript-spread-operator-to-infinity-and-beyond-9a3222bd944e?source=collection_archive---------30----------------------->

![](img/3f2a9867fce1c44e2d8fda706992bc03.png)

照片由 [Pexels](https://www.pexels.com/photo/space-shuttle-launch-during-nighttime-796206/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Edvin Richardson](https://www.pexels.com/@nivdex?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

三个点可以在一个句子中产生巨大的差异。在 JavaScript 中，这并不是例外。在 JavaScript 中，spread 运算符用三个点来标识。这种语法非常强大，因为它允许您以非常简单和容易的方式执行大量不同的任务。

让我们来看一些如何在 JavaScript 上使用这种语法的例子。

# 将数组作为参数传递

spread 操作符允许您获取一个数组，并以这样的方式分解它，现在您可以从数组中取出每个值，并可以作为参数传递给函数。

让我们从假设您有下面的代码开始。一个函数，它接受三个参数，并用逗号分隔它们，并记录一个包含三个元素的数组。

```
const fun = (a, b, c) => console.log(`${a}, ${b}, ${c}`);
const arr = [0, 1, 2];
```

你可以用一种简单但不理想的方式来做这件事。也就是说，通过索引从数组中取出每一个元素，并将它们作为参数传递。

```
const fun = (a, b, c) => console.log(`${a}, ${b}, ${c}`);
const arr = [0, 1, 2];fun(arr[0], arr[1], arr[2]);
```

通过使用 spread 语法，有一种更优雅(也更安全)的方式可以做到这一点。如您所见，当使用 spread 操作符时，现在它将所有三个元素作为参数传递给 *fun* 函数，不再需要担心索引了。

```
const fun = (a, b, c) => console.log(`${a}, ${b}, ${c}`);
const arr = [0, 1, 2];fun(...arr);
```

# 复制对象

spread 操作符的另一个有用的用例是，您可以使用它来复制对象。假设你想复制一个 *obj1* 复制到 *obj2* 。最后，您希望将属性 *a* 在 *obj2* 上的值更改为 3。

你的第一个想法*可能是*做以下事情…

```
const obj1 = { a: 1, b: 2, c: 3 };
const obj2 = obj1;obj2.a = 3;console.log(obj1);
console.log(obj2);
```

在这种情况下，尽管您认为您正在将 *obj2* 的值 a 更改为 3，但当您实例化 *obj2* 时，您是在为 *obj1* 分配一个引用，而不是进行复制。

你*可能*也想过手动复制属性。虽然这个例子可以工作，但是它不能伸缩。这是因为如果您最终向 *obj1* 添加更多的属性，这可能会导致将来的错误，因为需要手动复制对象。

```
const obj1 = { a: 1, b: 2, c: 3 };
const obj2 = { a: obj1.a, b: obj1.b, c: obj1.c };obj2.a = 3;console.log(obj1);
console.log(obj2);
```

还有其他方法可以让我们安全的点他，但是我们现在将集中在如何使用 spread 操作符。在这个例子中，你可以将 *obj2* 实例化为等于 *obj1* ，但是你要用花括号将它括起来，并在它前面使用 spread 运算符。

```
const obj1 = { a: 1, b: 2, c: 3 };
const obj2 = { ...obj1 };obj2.a = 3;console.log(obj1);
console.log(obj2);
```

在最后一段代码中，正如您所看到的，我们现在做的是利用 spread 操作符来构建 *obj2* 。这将创建一个与 *obj1* 相同的新对象。

# 串联数组

现在让我们假设你想把两个数组连接成一个数组。有多种方法可以做到这一点。

使用 spread 操作符来实现这一点也非常简单。首先创建一个新的数组，在其中插入两个数组，并用 spread 操作符作为它们的前缀。

```
const a = [1, 2, 3];
const b = [4, 5, 6];const c = [...a, ...b];console.log(c);
```

正如您所看到的，spread 运算符在多个领域大放异彩，它使得用 JavaScript 编写代码变得更加简单，并且在某些情况下不容易出错。尽情享受吧！