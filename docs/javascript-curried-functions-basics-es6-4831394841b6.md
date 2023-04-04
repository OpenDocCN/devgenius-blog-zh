# Javascript 支持的函数基础[ES6]

> 原文：<https://blog.devgenius.io/javascript-curried-functions-basics-es6-4831394841b6?source=collection_archive---------10----------------------->

![](img/5115ebf104b19a17a5b00e89707d2b83.png)

> 使用 ES6 符号快速查看 JS 中的 curried 函数的内容、原因和方式。

## 什么是咖喱功能？

一个 curried 函数本质上与一个常规函数相同，除了一点不同——***它可以连续地/一次一个地接受多个参数。*** curried 函数不是用所有的参数调用一个函数并返回结果，而是一次用一个或多个参数调用一个 curried 函数，在最终返回结果之前返回一个可以接受下一个参数的函数。

## 如何编写一个 Curried 函数？

让我们用一个经典的例子来说明将两个数`x`和`y`相加的方法。我们可以把它写成一个普通函数；

```
const add = (x,y) => x + y;
```

像这样调用这个方法。

```
let result = add(5,6); 
console.log(result) // 11
```

Curried 函数与常规函数非常相似，但是它们没有在方法签名中使用括号，而是使用了更多的箭头符号。这个函数的 curried 版本如下所示:

```
const add = x => y => x + y;
```

注意现在有两个`=>`箭头。不要慌！这仅仅意味着，如果我们调用只有一个参数的`add`函数(在本例中是`x`)，我们将**得到一个返回的函数，而不是一个值**。现在我们可以用各种方式调用这个 curried 函数。如果我们调用所有的参数，我们可以立即得到值。在调用一个函数后使用`()`用新指定的值再次调用那个函数。

```
let result = add(5)(6); 
console.log(result) // 11
```

如果我们用一个单独的参数调用 curried 函数，这毕竟是它的聚会把戏，我们得到返回给我们的函数；

```
let partialResult = add(5);
console.log(partialResult);// response of that console.log(partialResult)// function (y) {
//    return x + y;
//  }let fullResult = partialResult(6);
console.log(fullResult); // 11
```

在调用没有所有参数的 curried 函数后返回的函数，不出所料，被称为**部分函数**。分部函数将具有我们传递给它的任何值，这就是为什么我们可以一个接一个地添加参数，而不必担心丢失任何先前传递的值。

Curried 函数不仅限于两个参数— **您可以拥有任意多的参数！**

```
const add = x => y => z => x + y + z;let call1 = add(3)(4);let call2 = call1(5);console.log(call2); // 12
```

## 为什么要使用 Curried 函数？

万岁，我们现在可以理解和编写 curried 函数，但是你可能会问**为什么我需要使用它，它有什么好处？** 事实是，对于像将两个数相加的方法这样简单的事情来说，*阿谀奉承可能有点过头了*。但这并不意味着它没有用途。

Currying 是**函数式编程**的一种方法，函数式编程是一种编程范式，涉及使用纯组件和函数式组合编写声明性代码。所以如果你喜欢，你会经常使用奉承。

真实世界的函数很可能比我们的`add`方法复杂得多。如果你一直需要用相同的参数调用一个函数，那么把参数分开并单独调用它们是有意义的。此外，逢迎通常会使您的代码看起来更具声明性，更易于阅读——所以当需要审查拉取请求时，您的同事会感谢您。

Currying 是一个相对简单的概念，有更具体的用例，但是理解它的基础知识以添加到您的技巧盒中是值得的。🎉🎉🎉

*如果您喜欢这篇文章或觉得它有用，请随意。或者，你可以在 Medium* [*这里*](https://jamesmbrightman.medium.com/membership) *支持我，或者给我买一杯* [*咖啡*](https://ko-fi.com/jamesbrightman) *！非常感谢所有的支持。*