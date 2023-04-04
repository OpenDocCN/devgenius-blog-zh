# 理解 Javascript 中两倍等于和三倍等于的区别。

> 原文：<https://blog.devgenius.io/understanding-the-difference-between-double-equals-vs-triple-equals-in-javascript-9dfc8c4c36df?source=collection_archive---------9----------------------->

![](img/92fb729994a8cb07c1ea1c85ebbb7d82.png)

照片由 [Serpstat](https://www.pexels.com/@serpstat-177219?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 从 [Pexels](https://www.pexels.com/photo/apple-devices-books-business-coffee-572056/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

如果你不确定 JavaScript 中==和===的区别，你并不孤单。很多人仍然很难理解 javascript 中' == '和' === '之间的区别。

首先，让我们来了解一下什么是二等和三等:

**双等号(==):**

> 双相等的官方名称是抽象相等比较运算符或松散相等。它将比较两个元素，而不考虑它们的数据类型。它还会在执行比较之前将变量值转换为相同的类型。这就是所谓的[型强制](https://developer.mozilla.org/en-US/docs/Glossary/Type_coercion)。

**三重等于(===):**

> *三重等于被称为严格相等比较运算符或恒等运算符，但它将通过值和数据类型来比较两个元素。严格运算符在执行比较之前不会将变量值转换为相同的类型，而是仅当两个变量的值和类型在比较中相同时才返回 true。*

```
console.log(10 == "10"); //trueconsole.log(10 === "10"); //false
```

**注意:**当使用双等号时，它返回 true，因为在进行比较之前，字符串 10 被转换为数字 10。而 triple equals 比较类型和值并返回 false。

让我们来看看这个例子:

```
const myAge = 30;const yourAge= 30;console.log(myAge);  //30;console.log(yourAge);  //30;console.log(myAge == yourAge); //trueconsole.log(myAge === yourAge); //true 
```

请注意，double equals 和 triple equals 返回 true，因为两个变量具有相同的数据类型和值，即 30；

让我们考虑另一个例子:

```
const myAge = 30;const yourAge= "30";console.log(myAge);  //30;console.log(yourAge);  //30;console.log(myAge == yourAge); //trueconsole.log(myAge === yourAge); //false
```

注意区别，这一次 triple equals 返回 false。为什么，是因为 myAge 是一个值为 30 的整型变量，yourAge 变量是一个字符串值为“30”的字符串变量。

```
const fName= "Amy";const lName= "Cruz";console.log(fName);  //Amy;console.log(lName);  //Cruz;console.log(fName == lName); //trueconsole.log(fName === lName); //true
```

请注意，两倍等于和三倍等于是真的，因为它们都是相同的数据类型和值，都是字符串。

# 相等运算符和其他引用类型

到目前为止，我们一直在探索使用[原始类型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)的等式运算符。

像[数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)这样的引用类型呢？

```
const num1 = [1,2,3];const num2 = [1,2,3];console.log(num1 ==  num2); // falseconsole.log(num1 === num2); // false
```

注意:你不能像我们对基本类型那样比较两个有相同内容的数组。尽管数组内容相同，但它们的值本质上是不同的。这同样适用于对象和其他引用类型。

# 那么我应该用哪一个呢？

**三倍等于优于两倍等于。**

一般情况下，建议使用“===”运算符，因为它从不进行类型转换，我们做的是精确比较，因此它总是产生正确的结果。

感谢阅读。😊😊😊😊