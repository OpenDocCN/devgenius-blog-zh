# Javascript 很奇怪，是吗？

> 原文：<https://blog.devgenius.io/javascript-is-weird-or-is-it-4228e5211022?source=collection_archive---------23----------------------->

![](img/0c4fae0c2f578fb20ccaebb192812be9.png)

[精诚传媒](https://unsplash.com/@sincerelymedia?utm_source=medium&utm_medium=referral)在[Unplash](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄的照片

Javascript 是一种出身卑微的编程语言，主要用于在网站上进行愚蠢的交互，现在已经无处不在，其应用范围从物联网到人工智能和机器学习，根据 [Stack Overflow 的开发者调查](https://insights.stackoverflow.com/survey/2020#most-popular-technologies)连续 8 年被评为最受欢迎的编程语言。如果你能想到这一点，Javascript 可能就能做到。

尽管它很受欢迎，但许多人喜欢讨厌 Javascript，他们似乎认为它很奇怪，超级古怪，甚至不可预测。这种说法非常普遍，以至于很多博客甚至建议 Javascript 程序员完全避免像`Types`和`Coercion`这样的语言，因为“它会咬你的一口”。当我刚开始学习的时候，我同意其中的一些观点，但是我对这门语言了解的越多，我就越能理解和利用很多人所说的它的缺点。

Javascript 并不完美(没有一种编程语言是完美的)，但它绝不是不可预测的。Javascript 规范明确定义了该语言的行为，对 Javascript 内部工作机制的深入理解将使程序员能够利用其特性来构建复杂且可扩展的应用程序。

在本文中，我们将尝试解释 Javascript 中某些“奇怪之处”背后的原因，我希望这能鼓励读者更深入地研究 Javascript，并真正欣赏这种语言的强大功能。

不久前，我在网上找到了这些幻灯片，我非常喜欢尝试找出这些问题的解决方案，所以我们将尝试在这里一起做

[](https://slides.com/chrisoncode/javascript-is-weird) [## JavaScript 很奇怪。我喜欢 JavaScript

### 用幻灯片创建的演示文稿。

slides.com](https://slides.com/chrisoncode/javascript-is-weird) 

# 强迫

**对**`**Strings**`**`**Numbers**`进行算术运算**

```
1 + "1" // results in "11"9 + '99' + 100 // results in "999100"'hello' + 'goodbye' // results in "hellogoodbye"12 / "6" === 2 // true12 - "6" === 6 // true12 * "6" === 72 // true12 + "6" === 18 // false
```

**结果可能会让一些人感到惊讶，但这是非常简单的。当在 Javascript 中对原语执行加法操作时，如果任何一个值是字符串，编译器将调用两个值的本机`ToString`实现，将非字符串强制转换为字符串，并执行字符串串联，如果是减法、乘法和除法等其他算术运算，将调用`ToNumber`实现。**

****数组上的算术运算****

```
["chris", "kapehe"] + ["mo", "bella"] // "chris,kapehemo,bella"["chris", "kapehe"] - ["mo", "bella"] // NaN
```

**这是一个更有趣的例子，也是造成混乱的主要原因，但是同样，通过应用 Javascript 规范中的算法，很容易理解。**

```
["chris", "kapehe"] + ["mo", "bella"]/**
* When the Javascript compiler comes across this expression,
* it invokes the native ToPrimitive implementation on both sides
* of the expression and the expression becomes;
*/"chris,kapehe" + "mo,bella"// And from what we know about the addition operations on strings, this results in a string concatenation;"chris,kapehemo,bella"// For the subtraction case, we know that the native ToNumber implementation will be invoked resulting in NaN - NaN // NaN
```

**参见:**

**[https://www.ecma-international.org/ecma-262/5.1/#sec-11.6](https://www.ecma-international.org/ecma-262/5.1/#sec-11.6)**

**[https://www.ecma-international.org/ecma-262/5.1/#sec-11.5](https://www.ecma-international.org/ecma-262/5.1/#sec-11.5)**

**`**==**` **vs** `**===**`**

**`==`操作符是 Javascript 中另一个有着不好名声的功能。无数的博客帖子和教程都建议不惜一切代价避开操作员，只使用`===`，因为这样更“安全”,因为它不会尝试任何形式的强制。**

```
new Array(3).toString() === ",," // true',,' === ['', '', ''] // false',,' == ['', '', ''] // true
```

**这里的第一种情况非常简单，因为我们已经知道在数组上调用`ToString`会产生什么结果。让我们仔细看看第二种和第三种情况；**

```
/**
* This behaves as most would expect, a String is not the same as an * Array so this should be false
*/',,' === ['', '', ''] // false/**
* This might come as a surprise to most people but it is quite
* straightforward. When the Javascript compiler encounters this, 
* it tries to get the Right-hand side and Left-hand side to be of
* the same type so it invokes ToPrimitive on the Array
*/',,' == ['', '', ''] // true// this then becomes',,' == ',,'// And now since both sides of the expression are of the same type and value, we get `true` as the result.
```

**对强制有了深入的了解后，很明显`==`操作符确实比`===`更强大，并且当类型匹配时，它的行为与`===`操作符相同。**

**一个典型的例子是当我们需要检查`null`或`undefined`时`==`会发光**

```
// Instead of doing this
if (someValue === null || someValue == undefined)// We could simply do
if (someValue == null)//or
if (someValue == undefined)
```

**参见:【https://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3 **

# **范围和吊装**

****范围****

**Javascript 中的作用域概念甚至会让有经验的 Javascript 开发人员感到困惑，几乎每个编写 Javascript 的人都曾经历过与 Javascript 的作用域系统有关的事情。词法范围可能是 Javascript 最好的特性之一，很好地理解它的工作原理可以增强开发人员解决问题的能力。**

**让我们看看幻灯片中的几个例子；**

```
// Since `someVariable` has not yet been declared within this scope, an error is thrownconsole.log(someVariable) // Uncaught ReferenceError/**
* The `typeof` operator is special in that it can access an 
* undeclared variable without throwing an error. This means that 
* the `typeof` operator will always return a string no matter what
*/

console.log(typeof myVariable) // "undefined"/**
* Since we do not get block scoping with the `var` keyword, 
* the declaration of the `x` variable in the `if` block propagates 
* all through the function scope and the new value of `x` is 10
*/function doSomethingCool() {
  var x = 5;      if (true) {
    var x = 10;     
    console.log(x); // 10
  } console.log(x); // 10
}doSomethingCool();/**
* With the `let` keyword, we now have block scoping and so 
* within the block scope created by the `if` statement, 
* we have a variable `x` whose value is 10
* while the value of `x` remains 5 in the outer function scope
*/function doSomethingCool() {
  let x = 5;      if (true) {
    let x = 10;     
    console.log(x); // 10
  } console.log(x); // 5
}doSomethingCool();
```

****吊装****

**下面这个例子是 Javascript 中提升的经典例子。它通常被描述为 Javascript 将某些代码移动到特定范围的顶部，但这并不完全准确。**

**更好的理解方式是 Javascript 对代码进行两次传递，第一次传递设置范围、处理正式声明等，第二次传递执行代码。**

```
/**
* In the 'processing' phase, we skip the invocation of `sayWhatup` 
* and process the formal declaration of the `sayWhatup` function
* then at the execution stage, we find that `sayWhatup` has already * been declared and so we do not get a ReferenceError.
*/sayWhatup(); // alertfunction sayWhatup() {
    alert('whatup')
}
```

**这里有另一个例子可以更清楚地说明这一点**

```
/**
* In the 'processing' phase, we skip the invocation of `log` and 
* process the formal declaration of the `message` variable. 
* Since variable assignment happens during the execution stage, 
* `message` defaults to undefined, and that is what we see 
* when the code is executed.
*/console.log(message); // undefinedvar message = 'HEY everybody!';
```

**花点时间想出这里的其他例子:【https://slides.com/chrisoncode/javascript-is-weird】T2，看看你是否能想出来。**

**Javascript 是一种令人惊叹的语言，花些时间理解它是如何工作的，阅读说明书，它会帮助你体会到这种语言到底有多强大。**