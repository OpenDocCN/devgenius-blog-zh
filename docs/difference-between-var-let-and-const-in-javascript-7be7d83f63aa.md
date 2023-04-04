# JavaScript 中的 var、let 和 const

> 原文：<https://blog.devgenius.io/difference-between-var-let-and-const-in-javascript-7be7d83f63aa?source=collection_archive---------17----------------------->

## JavaScript 变量声明及其范围快速指南。

![](img/4758f0d0cc91aea861236b719015d137.png)

由[克里斯托弗·罗宾·艾宾浩斯](https://unsplash.com/@cebbbinghaus?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

本文将讨论 *var，let，*和 *const* 的区别，它们的适用范围，以及根据具体情况，它们之间的优劣。

当我们使用编程语言构建应用程序时，我们必须清楚地了解变量声明。变量是存储数据值的容器。范围定义了变量的可见性。有了 ES6 的新特性，JavaScript 增加了新的变量声明，其中 ***let*** 和 ***const*** 另外还有 ***var*** 。

## ***1。var***

> var 语句声明了一个**函数作用域的**或**全局作用域的**变量，**可选地将其初始化为一个值。 [*来源*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)**

根据定义， ***var*** 是函数作用域或全局作用域的变量。

函数作用域？
JavaScript 的每个函数都会创建一个新的作用域。当变量在函数内部定义时，它只在函数内部可见。它在我们的视线之外是无法接近的。

**全局作用域？**
当一个变量被声明在一个函数之外时，它就变成了全局变量，可以被网页上的所有脚本和函数访问。

下一个是*var*初始化是可选的。默认值为 ***未定义*** 。同样**重复变量**由*变量*声明**未发生错误**。现在我们举例讨论。变量声明在任何代码执行之前被处理。这叫吊装。因此，总是在变量作用域的顶部声明变量。

图 1(示例 1.js)

```
undefined
inside function x: 1
inside function y: 2
outside function x: 1
```

在 *Example1.js* *var* 声明中，变量 *x* 被初始化为 1。这里 x 是全局变量。
变量 *y* 使用*变量*声明初始化为 2。它的作用范围是函数*打印*。如果试图在函数之外访问 y，则生成错误 **ReferenceError: y 未定义。**

当在全局上下文中使用 var 声明变量时，属性描述符**不能** **改变**或**删除**。

> JavaScript 有自动内存管理，在全局变量上使用 delete 操作符是没有意义的。[来源](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)

图 2(示例 2.js)

```
inside the function x: 2
inside the function y: undefined
outside the function x: 1
```

## 2.让

> let 语句声明了一个**块范围的局部变量**，可选地将其初始化为一个值。[来源](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone)

*var* 关键字是将变量定义为作用域为全局或局部的一个整函数，在*中让* **变量的作用域限于一个 block 语句**(图 3) block 可以是{}，for 循环，if -else 等。

图 3(示例 3.js)

```
inside the code block of printVar: 2
inside the printVar: 2
inside the code block of printLet: 2
inside the printLet: 1
```

接下来，讨论第二个，*让*是**错误发生**，当它在变量被声明 d 之前被访问**时。该变量从块的开始直到初始化被处理都处于“时间死区”中。但是*变量*由于吊装原因在相同情况下被赋予**未定义**。**

图 4(示例 4.js)

```
inside printVar before declared: undefined
inside printVar after declared: 1
 console.log(“inside printLet before declared: “+ y);
ReferenceError: Cannot access 'y' before initialization
```

第三个是，使用 let 在**同一个块或函数**中声明重复变量出现**语法错误。**但是*变量*允许**在同一块中重复变量**。

图 5(示例 5.js)

```
let y = 1;
 ^
SyntaxError: Identifier ‘y’ has already been declared
```

当我们要用一个单独的块编写 switch 语句，并在每种情况下都使用 *let* 声明变量时，会产生一个语法错误。解决方案是在每个 case 子句中添加一个块(图 6)。

图 6(示例 6.js)

## **3。常量**

> 常量是**块范围的**，很像使用 let 关键字定义的变量。一个**常量**的值不能通过重新赋值来改变，它**也不能被** **重新声明**。[来源](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

*常量*由*更改为*不可重新分配。*常量*必须初始化。正常情况下，*常量*变量用大写字母声明。

图 7(示例 7.js)

*const* 块范围可以是全局的，也可以是局部的。与*变量*不同，*常量*变量不是窗口对象的属性。在同一个块中重新声明一个 *const* 变量会抛出一个错误。

在 example8.js 中，PI 被声明为常量，并全局赋值为 3.14。在 *if* 条件下，使用 let 创建了一个 PI 变量。它是一个非常量变量，如果阻塞，其范围为*。*

图 8(示例 8.js)

```
3.14
3.142857142857143
3.14
```

**结论**

当我们总结 *var* 、 *let* 和 *const* 的区别时，

***var***
函数作用域或全局作用域。

可选初始化。默认值是未定义的。

重复变量未出现错误

在声明变量之前访问变量时未定义。

变量是在全局上下文中使用 var 声明的，因此不能更改或删除属性描述符。

***让***

锁定范围的局部变量。

可选地将其初始化为一个值。

在声明变量之前访问，出现错误。

不允许在同一个块或函数中有重复的变量。

***常量***

块范围的。

常量不能通过重新分配来更改。

不能重新申报

常量变量不是窗口对象的属性。

**参考文献**

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements) [## 声明和宣言

### JavaScript 应用程序由具有适当语法的语句组成。一条语句可能跨越多行…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements) [](https://tylermcginnis.com/var-let-const/) [## JavaScript 中的 var vs let vs const

### 在这篇文章中，你将学习两种在 JavaScript (ES6)中创建变量的新方法，let 和 const。一路上我们会寻找…

tylermcginnis.com](https://tylermcginnis.com/var-let-const/)