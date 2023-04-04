# 你可能忽略的 7 个 JavaScript 编码技巧

> 原文：<https://blog.devgenius.io/7-javascript-coding-tips-you-might-ignore-2d44a1f3788a?source=collection_archive---------8----------------------->

## 它们很简单，但是很容易忘记

![](img/8be80d37529bf9d601bf5a22016a460f.png)

[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

有很多文章告诉你 JavaScript 的编码技巧，这篇文章也不例外。但是这篇文章可能是你忽略或者忘记的东西。我们一起快速看一下，真的很简单。

# 1.同时履行承诺

`async...await`大大提高了 JavaScript 的异步处理能力。我们可以像编写同步代码一样编写异步代码，而不用关心内部实现。但是如果两个或多个异步任务彼此不相关，那么我们可以使用`[Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)`或`[Promise.allSettled](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)`来并发执行它们，并在返回中获得它们各自的值。

# 2.`Object.keys`对`Object.values`对`Object.entries`

一些开发者一直在使用`Object.keys`来获取对象自身的可枚举属性，然后使用 key 来访问相应的值。这种情况其实可以换成`Object.entries`。所以合理使用这三种方法，让我们的编码更好。

```
const obj = {
  a: 'some',
  b: 42,
};// [ 'a', 'b' ]
console.log(Object.keys(obj));// [ 'some', 42 ]
console.log(Object.values(obj));// [ [ 'a', 'some' ], [ 'b', 42 ] ]
console.log(Object.entries(obj));
```

# 3.使用`Array.prototype.at()`或`String.prototype.at()`

ECMAScript 2022 中新的`at()`方法解决了一个非常实际的问题，即所有基本的可索引类(Array、String、TypedArray)都可以“负索引”，就像 Python 中一样。这是一种更通用的方法，允许相对索引。

```
const arr = ['1', '2', '3'];// '3'
console.log(arr[arr.length - 1]);// '3'
console.log(arr.at(-1));
```

更多 ECMAScript 2022 新特性:

[](/ecmascript-2022-is-officially-released-what-should-we-pay-attention-to-5e207ed61a46) [## ECMAScript 2022 正式发布，需要注意什么？

### 有没有带来什么有趣的东西？

blog.devgenius.io](/ecmascript-2022-is-officially-released-what-should-we-pay-attention-to-5e207ed61a46) 

# 4.`||`或`??`

这两者是有区别的。`||`是判断左侧值是否为[falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy),`??`是判断左侧值是否为 [nullish](https://developer.mozilla.org/en-US/docs/Glossary/Nullish) 。所以`??`可以看作是`||`的一个特例。

如果您在判断中使用高频 [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) 值，如`''`或`0`，一定要注意这一点，以避免意想不到的结果。

`??`具有第五低的[运算符优先级](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)，直接低于`||`并直接高于[条件(三元)运算符(条件？exprIfTrue : exprIfFalse)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) 。

`[??=](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_nullish_assignment)`或`[||=](https://developer.mozilla%20.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_OR_assignment)`类似，只是有一个赋值操作。

# 5.使用命名参数

在定义一个函数时，如果你传递了太多的参数，比如超过 4 个项，为了更好的维护，最好对参数进行分类。

```
// NO
function example(arg0, arg1, arg2, arg3, arg4) {}// YES
function example(arg0, arg1, arg2, relatedParam = { arg3: 3, arg4: 4 }) {}
```

# 6.使用`padStart`和`padEnd`

这两个方法可以将字符串填充到原始字符串，并使其达到指定的长度。`padStart`从开始垫起，`padEnd`从结束垫起。

例如，当我们想用`0`填充日期时:

```
const fakeTimestamp = 1657040882000;// 2022-07-05T17:08:02.000Z
const date = new Date(fakeTimestamp);**// NO
function padZero(e) {
  if (String(e).length === 1) {
    return `0${e}`;
  }
  return e;
}****// YES
function padZero(e) {
  return String(e).padStart(2, '0');
}**const month = date.getMonth() + 1;
const day = date.getDay();
const hours = date.getHours();
const minutes = date.getMinutes();console.log(
  `${padZero(month)}/${padZero(day)} ${padZero(date.getHours(hours))}:${padZero(minutes)}`,
);
```

# 7.`toString`的一些提示

比较两个数组前后是否相同时，常规的做法是遍历每一项，然后依次比较。例如:

```
const isEqual = (prev, cur) => prev.every((e, i) => e === cur[i]);// false
console.log(isEqual([1, 2, 3], [1, 2, 5]));
// true
console.log(isEqual([1, 2, 3], [1, 2, 3]));
```

我们也可以用`Array.prototype.toString`把数组转换成字符串然后直接比较，这样在数组内部级别不变的情况下，也能高效的得到预期的结果。

```
const isEqual = (prev, cur) => prev.toString() === cur.toString();// false
console.log(isEqual([1, 2, 3], [1, 2, 5]));
// true
console.log(isEqual([1, 2, 3], [1, 2, 3]));
```

此外，我们还可以使用`Number.prototype.toString([radix])`中的可选参数`radix`将数字转换为指定的基数。`radix`的取值范围是 2 到 36。

例如，我们可以快速地将十进制转换成二进制或十六进制:

```
// 1010
console.log(Number(10).toString(2));
// a
console.log(Number(10).toString(16));
```

我们也可以结合`Math.random`来生成十六进制的随机值:

```
// 0.2b9f3601ffc91
console.log(Math.random().toString(16));
```

*感谢阅读。如果你喜欢这样的故事，想支持我，请考虑成为* [*中等会员*](https://medium.com/@islizeqiang/membership) *。每月 5 美元，你可以无限制地访问媒体内容。如果你通过* [*我的链接*](https://medium.com/@islizeqiang/membership) *报名，我会得到一点佣金。*

你的支持对我来说非常重要——谢谢。