# JavaScript 中的扩展运算符(…)

> 原文：<https://blog.devgenius.io/what-is-spread-operator-in-javascript-87233cd85bff?source=collection_archive---------32----------------------->

## 在 JavaScript 中使用 spread 运算符的快速指南。

![](img/ba517e064f41c12f691f5b5c886c1ef4.png)

Emile Perron 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

当我们遵循 JavaScript 代码时，我们可以看到有时有 3 个点(…)。这三个点是什么(…)。它是 Java Script 中的 spread 运算符。在本文中，我将讨论 JavaScript 中的 spread 操作符。

随着 ES6 的新功能，传播运营商是其中之一。让我们讨论它是什么？，为什么呢？、用例等等相关的例子。

根据 MDN

> Spread 语法允许在需要零个或多个参数(对于函数调用)或元素(对于数组文本)的地方扩展可迭代对象，例如数组表达式或字符串，或者在需要零个或多个键值对(对于对象文本)的地方扩展对象表达式。[来源](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

**传播算子是做什么的？**

它可以将数组或字符串中的项作为单独的参数或元素展开，并在对象中展开键值对。让我们用例子来讨论。

**我们在哪里使用它？**

扩展运算符用于

1.  函数调用
2.  数组文字或字符串
3.  对象文字

**1 .函数调用**

首先我们写一个函数来求两个数的和。

```
const numbers = [6,8];function findSum(num1, num2){
  console.log(num1+num2);  //14
}findSum(numbers[0], numbers[1]);
```

上面代码的输出是 14 **。**现在添加传播算子。我们可以添加 spread 运算符来传递 ***数字*** ，而不使用*数字[0]，数字[1]。*

```
const numbers = [6,8];function findSum(num1, num2){
 console.log(num1+num2);  //14
}findSum(…numbers);
```

输出，

```
14
```

***…numbers*** 将数组的元素传递给函数。在这里， *findSum()* 只接受数组的第一个和第二个元素。让我们尝试改进代码。

```
const numbers = [6,8,3];function findSum(...arg){
  console.log(arg);              //[ 6, 8, 3 ]               
  console.log(arg[0] + arg[1]);  //14
}findSum(...numbers);
```

输出，

```
[ 6, 8, 3 ]
14
```

这里， *findSum()* 也在**函数参数**中加入了扩展运算符。

**2 .数组文字或字符串**

这里我有奇数和偶数。我想把它们加到一个数组里。尝试..

```
const odds = [1,3,5];
const even = [2,4,6];const numbers = [odds, even]console.log(numbers);  //[ [ 1, 3, 5 ], [ 2, 4, 6 ] ]
```

输出，

```
[ [ 1, 3, 5 ], [ 2, 4, 6 ] ]
```

但是这里我不能得到预期的输出，这里没有一个没有嵌套数组的数组。但是在 Array.prototype.concat 中使用 concat intor*()*…

```
const odds = [1,3,5];
const even = [2,4,6];const numbers = odds.concat(even)console.log(numbers);     //[ 1, 3, 5, 2, 4, 6 ]
```

输出，

```
[ 1, 3, 5, 2, 4, 6 ]
```

现在尝试添加扩展运算符。

```
const odds = [1,3,5];
const even = [2,4,6];const numbers = […odds, …even]console.log(numbers);
```

输出，

```
[ 1, 3, 5, 2, 4, 6 ]
```

这里我们有一个数组，包含所有奇数和偶数元素。***concat()****和**展开运算符**都成功。*

*现在对比一下 ***推()*** vs ***传播算子****

```
*let odds = [1,3,5];
let updatedOdd = odds        
updatedOdd.push(7)console.log(updatedOdd);        //[1,3,5,7]
console.log(odds);              //[1,3,5,7]/* using spread syntax */let even = [2,4,6];
let updatedEven = even
updatedEven = […even, 8];console.log(updatedEven);       //[2,4,6]
console.log(even);              //[2,4,6,8]*
```

*根据上面的代码，那当我们尝试给 ***加 7 的时候【updatedOdd】****，*原数组(*)也被改变了。但是当我们尝试给 ***updatedEven*** 加 8 时，spread 运算符并不会影响到原来的数组(*)。***

***3 .对象文字***

> ***ECMAScript 提案(ES2018)的 Rest/Spread 属性向对象文字添加了 Spread 属性。它将自己的可枚举属性从一个提供的对象复制到一个新对象上。[来源](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)***

***带有扩展运算符的对象文字用于用新对象扩展现有对象。***

```
***studentBio={
 name: “Jenny”,
 age: “20”
}studentCollege={
 collegeName: “x”,
 collegeLevel: 4
}const studentDetails = {…studentBio, …studentCollege};
console.log(studentDetails)***
```

***输出，***

```
***{ name: ‘Jenny’, age: ‘20’, collegeName: ‘x’, collegeLevel: 4 }***
```

***这里所有可枚举的细节都被复制到合并的对象中。***student details***是 ***studentBio*** 和***student college***的副本。***

****结论****

**本文涵盖了 JavaScript 中 spread 操作符背后的一个基本主题。这里讨论了什么是扩展运算符，它用在什么地方。然后讨论带有实现代码片段的扩展操作符的行为。**

**[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) [## 扩展语法

### Spread 语法允许在零个或多个…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)**