# JavaScript 中一些和每一个的区别

> 原文：<https://blog.devgenius.io/difference-between-some-and-every-in-javascript-de3bbca334a1?source=collection_archive---------6----------------------->

## 了解 JavaScript 中的一些方法。

![](img/48cbf8fc400a25002840c269225af6f3.png)

[Jexo](https://unsplash.com/@jexo?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

*本文原载于我的博客*[*WebDevIdea*](https://webdevidea.com/blog/some-vs-every-in-javascript/)*。*

JavaScript 有很多有用的方法，我们可以用它们轻松地处理数组。这些 JavaScript 方法被称为[高阶函数](https://webdevidea.com/blog/javascript-map-filter-reduce/)。所以请记住，任何以另一个函数作为自变量的函数都称为高阶函数。

在本文中，我们将通过介绍两个高阶函数`some()`和`every()`之间的差异来了解它们。让我们开始吧。

# 一些方法

JavaScript 中的高阶函数`some()`与[数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)一起使用。它检查数组中的任何元素是否通过了我们提供的测试。如果数组中的一个元素通过测试，则返回`true`。如果没有一个元素通过测试，它将返回`false`。

方法`some()`将一个回调函数作为其参数。回调函数本身有三个参数:

*   数组元素(必需)。
*   元素索引(可选)。
*   数组本身(可选)。

让我们看一个实际的例子:

```
let numbers = [6, 7, 8, 9, 10];//Using ES5 syntax.
 numbers.**some**(function(number){ 
  return number > 8; 
 }); // returns true.//Using ES6 syntax.
numbers.**some**(number => number > 8); // returns true.
```

正如您在上面看到的，这里的方法`some()`检查数组中是否有大于 8 的数字。这是真的，因为我们有大于 8 的 9。这就是方法`some()`返回 true 的原因。

因此高阶函数`some()`遍历数组中的每个元素，直到找到通过测试的元素(大于 8)，然后返回 true。如果数组元素都不大于 8，它将返回 false。

除此之外，方法`some()`不改变原始数组`numbers`。

```
console.log(numbers); // output: [6, 7, 8, 9, 10]
```

下面是另一个返回`false`的例子:

```
let numbers = [6, 7, 8, 9, 10];numbers.**some**(function(number){
 return number < 6;
}); // returns false.//ES6 syntax.
numbers.**some**(number => number < 6); // returns false.
```

# 该方法每

方法`every()`也用于 JavaScript 中的数组。它检查数组中的所有元素是否都通过了我们提供的测试。如果数组中的所有元素都通过测试，它将返回`true`。如果至少有一个元素没有通过测试，它返回`false`。

方法`every()`也将回调函数作为其参数。回调函数本身有三个参数:

*   数组元素(必需)。
*   元素索引(可选)。
*   数组本身(可选)。

看看下面的代码示例:

```
let numbers = [6, 7, 8, 9, 10];//Using ES5 syntax.
 numbers.**every**(function(number){ return number >= 6; }); // returns true.

//Using ES6 syntax.
 numbers.**every**(number => number >= 6); // returns true.
```

在上面的例子中，方法`every()`检查数组中的每个数字是否都大于或等于 6。因为数组`numbers`中的所有数字都大于等于 6，所以函数返回`true`。

所以高阶函数`some()`遍历数组中的每个元素。如果至少有一个元素不大于等于 6，则返回`false`。但是如果所有的数组元素都通过测试，就会返回`true`。
除此之外，方法`every()`也不改变原来的数组`numbers`。

```
console.log(numbers); // output: [6, 7, 8, 9, 10]
```

这是另一个例子:

```
let names = ['John', 'John', 'Mehdi', 'John']; //ES5 syntax.
 names.**every**(function(name){ return name === 'John'; }); // returns false. //ES6 syntax.
 names.**every**(name => name === 'John'); // returns false.
```

正如您在上面看到的，我们使用了方法`every()`来检查是否所有的数组元素都有名称`John`。

因为我们在 names 数组中有另一个名字`Mehdi`，所以函数返回`false`。

# 结论

`some()`和`every()`是 JavaScript 中你应该知道的两个有用的数组方法。第一个检查数组中是否有元素通过了测试函数。另一方面，第二个函数(every)检查数组中的所有元素是否都通过了测试函数。所以这是 JavaScript 中这两个高阶函数的唯一区别。

感谢您阅读这篇文章。我们希望您觉得它有用。

*原载于 2021 年 3 月 28 日 https://webdevidea.com**的* [*。*](https://webdevidea.com/blog/some-vs-every-in-javascript/)