# 揭秘 Javascript 中的时态死区？

> 原文：<https://blog.devgenius.io/demystifying-temporal-dead-zone-in-javascript-e81103d5e41e?source=collection_archive---------5----------------------->

![](img/3984af53081cd911f5f487ad88448e12.png)

阿菲夫·库苏马在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

> 在我们开始理解**时态死区** e 之前，你应该理解 **Javascript 中的 [**提升**](https://rajeshi.medium.com/learn-hoisting-in-javascript-the-practical-way-a29741cf3eca) 。**你可以在**吊装** [**这里**](https://rajeshi.medium.com/learn-hoisting-in-javascript-the-practical-way-a29741cf3eca) 查看我的帖子。

# let vs var。

让我们看看下面的例子。

```
**CODE:**
console.log(fruit1);
console.log(fruit2);var fruit1 = "apple";
let fruit2 = "mango";
**OUTPUT:** undefinedReferenceError: Cannot access 'fruit2' before initialization
```

我们得到了未定义的“var ”,因为它被挂在执行上下文的内存组件中。类似地,“let”也被挂起，但是我们没有得到未定义的“ReferenceError”。

# 为什么让引用出错？

是，let 被提升了，但是它被分配到不同的内存空间中，与 var 不同。我们可以说**让**处于**时间死区**。

# Javascript 中的时间死区到底是什么？

在完全初始化之前，我们不能访问 **let** 变量。如果没有初始值给 **let** 声明，那么变量用 **undefined** 初始化，但是如果我们试图访问，它给出 **ReferenceError** 。只有当变量用一个值完全初始化时，我们才能访问它。let 在初始化之前不允许访问的这种性质被称为在**时间死区**中。

# 一个时间死区的结束。

当变量用一个值初始化时，时间死区结束。让我们看看例子，更好地理解。

```
**CODE:** let fruit = "apple";
console.log(fruit2);**OUTPUT:** apple
```

这里，我们成功地访问了 **let 变量**，因为我们在访问它之前用一个值初始化了它。所以它不在时间死区。但是如果我们可以在上面的行中有“console.log(fruit)”并在下面声明变量，那么由于时间死区，我们将得到作为 ReferenceError 的输出。

# Javascript 中的执行流和时间死区(TDZ)。

让我们看更多的例子来获得一个深入的概念。

## 在时间死区外调用函数

```
**CODE:** *// TDZ starts* const sayHello = () => console.log(greet);*// Within the TDZ if we access* greet throws *`ReferenceError`*let greet = "hello"; *// End of TDZ*sayHello(); *// called outside TDZ***OOUTPUT:** hello
```

## 在时间死区内调用函数

```
**CODE:** *// TDZ starts at beginning of scope* const sayHello = () => console.log(greet);sayHello();*// Accessing within the TDZ throws `ReferenceError`*let greet = "hello";*// End of TDZ***OUTPUT:** ReferenceError
```

通过上面两个例子，我们可以得出这样的结论:变量的时间死区取决于执行的时间，而不是代码的编写顺序。

# 要记住的最后一个音符。

**变量**在全局内存空间中被提升，在内存分配时被赋值**未定义**。但是**字母**被分配到单独的内存空间中，并且处于**时间死区**中，直到它用**值**完全初始化。

是的， **const** 也被分配到单独的内存空间中，并经过**时间死区**，就像 **let** 一样。

# 如果你想要更多这样的内容，请在媒体上关注我，订阅我的 YouTube 频道

# 有疑问吗？通过推特[联系我](https://twitter.com/izrajesh)