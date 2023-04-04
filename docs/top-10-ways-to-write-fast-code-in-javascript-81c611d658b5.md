# 用 javascript 编写快速代码的 10 大方法

> 原文：<https://blog.devgenius.io/top-10-ways-to-write-fast-code-in-javascript-81c611d658b5?source=collection_archive---------4----------------------->

## 感谢这些方式，你写的代码会被缩短，你写代码会更快。

![](img/21c8be7a30a31a245fdda1d3d0b2ca0c.png)

[胡安·戈麦斯](https://unsplash.com/@nosoylasonia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

JavaScript 快捷方式不仅仅加快了编码过程。它也加快了脚本的速度。因此，页面加载速度更快。快捷代码就像它们的长版本。它以更紧凑的方式执行长码的功能。您可以使用它们进行简单的代码优化。尤其是初学者，可以使用我下面提到的 javascript 快捷键。

# **1。十进制数字**

如果你经常在开发项目中使用大小数。这条捷径会很棒的。您不再需要键入零。你只需要用 e 符号代替它们。比如 1e5 就是在 1 位数后加 5 个零，等于 100000。

如果你在字母 e 的前面写一个数字，那就在零的前面写这个数字。比如说；22e3 等于 22000。

```
/* Shorthand */
var myVar = 1e5;

/* Longhand */
var myVar = 100000
```

# **2。增减**

增量快捷方式由两个+号组成。这意味着变量的值将增加 1。类似地，减量缩写由两个负号组成。这意味着变量的值将减少一。

这两个快捷方式只能用于数值数据类型。在循环中使用它非常重要。它最常用于 for 循环中。

```
/* Shorthand */
i++;
j--;

/* Longhand */
i=i+1;
j=j-1;
```

# **3。加、减、乘、除**

这是四种基本数学运算的捷径。类似于增减快捷方式。
在下面的例子中，变量 I 增加了 4。变量 j 减少了 7。变量 k 被乘以 5。变量 l 除以 3。

```
/* Shorthand */
i+=4;
j-=7;
k*=5;
l/=3;

/* Longhand */
i=i+4;
j=j-7;
k=k*5;
l=l/3;
```

# **4。确定字符位置**

CharAt()方法是最常用的字符串方法之一。将字符返回到指定位置。(例如，字符串中的第 7 个字符)。注意 CharAt()方法是从零开始的。

```
var myString = "Happy birthday";

/* Shorthand */
myString[4];

/* Longhand */
myString.charAt(4);
```

# **5。批量指定变量**

你不必一个一个地写它们来同时创建多个变量。使用 var(或 let)关键字就足够了。然后，您可以用逗号分隔要创建的变量。使用这个快捷方式，您可以指定未定义的变量和有值的变量。

```
/* Shorthand */
var i, j=5, k="Good morning", l, m=false;

/* Longhand */
var i;
var j=5;
var k="Good morning";
var l;
var m=false;
```

# **6。指定一个关系数组**

使用语法 var myArray = ["apple "、" pear "、" orange"]在 JavaScript 中声明数组是一项相对简单的任务。然而，声明一个关系数组有点复杂，因为在这里，你不必定义值，但你也必须定义键(键 0，1，2，3 等。在正常阵列中)。

关联数组是键/值对的集合。很长一段时间都是声明数组，然后逐个添加每个元素。但是，使用下面的缩写，您可以同时向关联数组声明它的所有元素。

在下面的示例中，myArray 关联语法指定了名人(关键字)的出生地(值)。

```
/* Shorthand */
var myArray  = {
  "Grace Kelly": "Philadelphia",
  "Clint Eastwood": "San Francisco",
  "Humphrey Bogart": "New York City",
  "Sophia Loren": "Rome",
  "Ingrid Bergman": "Stockholm"
}

/* Longhand */
var myArray = new Array();
myArray["Grace Kelly"] = "Philadelphia";
myArray["Clint Eastwood"] = "San Francisco";
myArray["Humphrey Bogart"] = "New York City";
myArray["Sophia Loren"] = "Rome";
myArray["Ingrid Bergman"] = "Stockholm";
```

# **7。指定对象**

对象声明的缩写关系序列的工作方式类似。但是，这里没有应该放在括号{}中的属性-值对，而不是键值对。

缩写语法的唯一区别是对象属性没有用引号括起来(在下面的示例中是 name、placeOfBirth、age、wasJamesBond)。

```
/* Shorthand */
var myObj = { name: "Sean Connery", placeOfBirth: "Edinburgh",
age: 86, wasJamesBond: true };

/* Longhand */
var myObj = new Object();
myObj.name = "Sean Connery";
myObj.placeOfBirth = "Edinburgh";
myObj.age = 86;
myObj.wasJamesBond = true;
```

# **8。使用**的条件运算符

条件(三元)运算符通常用作 if-else 语句的快捷方式。它由三部分组成:

条件
如果情况是真的怎么办(if)
如果情况是错的(else)
If—else 博客制作了一段 4–5 行的代码，通过下面显示的快捷方式，已经减少到 1 行。

```
var age = 17;

/* Shorthand */
var message = age >= 18 ? "Allowed" : "Denied";

/* Longhand */
if( age >= 18) {
  var message = "Allowed";
} else {
  var message = "Denied";
```

# **9。检查是否存在**

经常发生的情况是，你需要检查一个变量是否存在。“if presence”的缩写有助于您用更少的代码做到这一点。

```
var myVar = 99;

/* Shorthand */
if( myVar ) {
  console.log("The myVar variable is defined AND it's not empty
  AND not null AND not false.");
}

/* Longhand */
if( typeof myVar !== "undefined" && myVar !==  "" && myVar !== null
&& myVar !== 0 && myVar !== false  ) {
  console.log("The myVar variable is defined AND it's not empty
  AND not null AND not false.");
```

# **10。检查你的缺席情况**

语句“if presence”可以用来检查变量的不存在，方法是在前面加上一个感叹号。感叹号是运算符，在 JavaScript(和大多数编程语言)中并非不合逻辑。

因此，可以用 if(！idspnonenote)检查变量 myVar 是未定义的、null、null 还是 false。MyVar)符号。

```
var myVar;

/* Shorthand */
if( !myVar ) {
  console.warn("The myVar variable is undefined (OR) empty (OR)
  null (OR) false.");
}

/* Longhand */
if( typeof myVar === "undefined" || myVar === "" || myVar === null
|| myVar === 0 || myVar === false  ) {
  console.warn("The myVar variable is undefined (OR) empty (OR)
  null (OR) false.");
}
```

本文最初由侯赛因先生发表在《大众事物》杂志上

# 相关文章

[](https://medium.com/dev-genius/7-critical-tips-to-learn-programming-faster-802da8d604f2) [## 快速学习编程的 7 个关键技巧

### 在这篇文章中，我将告诉你七个技巧，让你更快地学习编程。

medium.com](https://medium.com/dev-genius/7-critical-tips-to-learn-programming-faster-802da8d604f2) [](https://medium.com/dev-genius/top-10-coding-software-languages-in-2021-aaf60e8d5596) [## 2021 年十大编码软件语言

### 你应该学习这些流行的软件语言

medium.com](https://medium.com/dev-genius/top-10-coding-software-languages-in-2021-aaf60e8d5596) [](https://medium.com/dev-genius/the-change-of-coding-language-softwares-by-time-cef89802afc6) [## 编码语言软件随时间的变化

### 从过去到现在的历史解释

medium.com](https://medium.com/dev-genius/the-change-of-coding-language-softwares-by-time-cef89802afc6) [](https://medium.com/dev-genius/what-is-matlab-why-we-need-it-d61e405ef419) [## Matlab 是什么？我们为什么需要它？

### 了解 Matlab 的基础知识

medium.com](https://medium.com/dev-genius/what-is-matlab-why-we-need-it-d61e405ef419) [](https://medium.com/dev-genius/understanding-principles-of-python-1c01eadd4e2f) [## 理解 Python 的原理

### Python 基础循序渐进

medium.com](https://medium.com/dev-genius/understanding-principles-of-python-1c01eadd4e2f) [](https://medium.com/dev-genius/50-basic-python-code-examples-e1a261c006f5) [## 50 多个基本 Python 代码示例

### 列表、字符串、分数计算等等..

medium.com](https://medium.com/dev-genius/50-basic-python-code-examples-e1a261c006f5)