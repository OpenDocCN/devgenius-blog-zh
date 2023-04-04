# 10 个最常被问到的 Javascript 问题，回答！[第一部分]

> 原文：<https://blog.devgenius.io/10-most-often-asked-javascript-questions-answered-5d2c516e8eb4?source=collection_archive---------35----------------------->

![](img/a734390227a5c907d6cd81a6f88825f7.png)

[Duomly —编程在线课程](https://www.duomly.com)

本文原载于[https://www . blog . duomly . com/10-most-popular-JavaScript-interview-question-and-answers-for-初学者/](https://www.blog.duomly.com/10-most-popular-javascript-interview-questions-and-answers-for-beginners/)

# 向初学者介绍 10 个最受欢迎的 Javascript 问题

在本文中，我收集了 10 个最常被问到的关于 Javascript 的问题，并决定逐一回答这些问题。

它们主要涵盖了 Javascript 基础知识，所以如果您刚刚开始学习这种编程语言，通读它们并理解基本概念是个好主意。

在这个列表中，你可以找到诸如闭包、承诺、提升或类等主题。

虽然知识不是很先进，但知道答案是很好的，因为有些答案在面试中经常被问到。

在每一种编程语言中，有些概念看起来很简单，但是对于初学者来说，理解起来并不容易。

所以，我更乐意向你描述和解释其他人问的所有事情，这可能对你也有帮助，你可以避免在搜索框中输入。

像往常一样，我为那些喜欢看阅读的人准备了视频版本。你会在每个问题下找到它。

开始吧！

# Javascript 中的闭包是什么？

[Javascript 中的闭包是什么？](https://youtu.be/Pn2VAebZPOk)

闭包是封闭在一起的函数的组合，其中内部函数可以访问它的变量和外部函数的变量。

我认为解释它最简单的方法是给你看一个代码示例。

```
function outer() {
  var name = 'Maria';
  function inner() {
    console.log(name);
  }
  inner();
}
outer();
// 'Maria'
```

在上面的代码中，可以看到`inner()`函数可以访问它的父函数变量 name。所以，如果你调用`outer()`函数，来自`inner()`函数的`console.log()`将返回名称变量`Maria`。

然而，它可以访问外部函数参数对象，但通常内部函数有自己的参数对象，这掩盖了外部函数参数对象。

让我们看看这个例子，我们将使用一个箭头函数创建闭包。

```
function outer(a, b) {
  const inner = (a, b) => console.log(a, b);
  inner(1, 2);
}
outer('Alice', 'Mark');
// returns 1, 2
```

我们使用闭包的主要原因是为了返回能够返回其他函数的函数。

# Javascript 中的 DOM 是什么？

[Javascript 中的 DOM 是什么？](https://youtu.be/euYe7mY33r0)

DOM 是一个文档对象模型，它是网站的面向对象的表示。可以使用 Javascript 修改它。

使用 Javascript 你可以操作 DOM 元素，比如颜色，位置，大小。对于选择页面的特定元素，Javascript 提供了一些函数来简化:

**getElementById()** —通过 Id 属性选择元素，

**getElementByName()** —通过 Name 属性选择元素，

**getElementsByTagName()**—选择所选标签的所有元素，

**getElementsbyClassName()**—选择具有特定类名的所有元素，

**querySelector()** —通过 CSS 选择器选择元素。

Javascript 还提供了其他方法来操作元素，而不仅仅是选择它们，比如`appendChild()`或`innerHTML()`。

除此之外，使用 Javascript，我们可以处理事件和样式。

# Javascript 中的 Promise 是什么？

[Javascript 中的 promise 是什么？](https://youtu.be/hFA6jUmRoMs)

承诺与异步编程一起使用，它用于启动一个动作，这个动作需要时间来解析和返回值。

有了承诺，动作可以在后台启动和完成，而不需要停止应用程序的其他操作。

它改善了许多 web 和移动应用程序的性能和用户体验。

承诺可能处于三种状态:待定、以值解决或因错误被拒绝。

如果承诺得到解决，我们可以调用`then()`方法，并使用返回值执行一个操作。如果承诺被拒绝，我们可以使用`catch()`方法来处理错误。

其他处理异步编程的方法有`async/await`和`callbacks`。

# Javascript 中的原型是什么？

[Javascript 中的原型是什么？](https://youtu.be/_UsvBouYLgo)

Javascript 对象从原型继承方法和属性，而`Object.prototype`位于继承链的顶端。

Javascript `prototype`关键字也可以用来给我们的构造函数添加新的值和方法。

让我们看看代码示例。

```
function Animal(name, kind, age) {
  this.name = name;
  this.kind = kind;
  this.age = age;
}Animal.prototype.ownerName('Mark');
```

您可以看到，使用原型，我们能够将`ownerName`属性添加到我们的`Anima()`构造函数中。

# Javascript 中的提升是什么？

[Javacsript 中的提升是什么？](https://youtu.be/hQfJo5-vdSQ)

提升是将所有声明的变量和函数提升到局部作用域顶部或全局作用域顶部(如果它们被放在全局作用域中)的机制。

在 Javascript 中，变量可以在使用后声明。

提升用于避免未定义的错误，因为否则，带有变量或函数的代码可能会被执行，但它没有被定义。

记住首先声明你的变量，以确保你的代码不会有任何未定义值的问题。

这里有一个例子向你展示它是如何工作的。

```
// What you see
name = 'Ted';
console.log(name);
var name;
// returns 'Ted'// What happens in a background
var name;
name = 'Ted';
console.log(name);
// returns 'Ted';
```

虽然您将使用 var 创建一个变量定义，但它将在每一行中被初始化为未定义。

let 和 const 有点不同。直到初始化真正发生的那一行，变量才被初始化。

所以，它不同时调用任何未定义的。

另外，重要的是要记住，当你声明 const 时，有必要同时初始化它，因为不可能改变它。

# Javascript 中的对象是什么？

对象是 Javascript 的一个非常重要的元素，JS 中的几乎所有东西都是对象。
当变量是值的容器时，对象可以有多个值，并且可以赋给一个变量。

对象中的值被写成名称:值对。对象由属性和方法组成。

属性只是简单的值，方法是可以在对象上执行的操作。

让我们看一下对象示例。

```
var student = {
  firstName: 'Alice',
  lastName: 'Jones',
  age: 21,
  sayHi: () => {
    return 'Hi, I am ' + this.firstName;
  }
}
```

在上面的代码中，你可以看到 student 对象，它有三个属性和一个方法。

# Javascript 中的函数是什么？

[Javascript 中的函数是什么？](https://youtu.be/QVPe565XOkA)

Javascript 中的函数是一段代码，用于执行一项任务。当函数被调用时，它被执行。

函数是用 function 关键字或常量来定义的。函数可能有名字，也可能是匿名的。

当我们定义函数时，我们可以在函数名后面的括号中添加一些参数。

当我们调用函数时，圆括号中传递的值称为参数。

让我们看看 Javascript 函数的代码示例。

```
function calculate(x, y) {
  return x * y;
}calculate(2, 5);
```

# Javascript 中的纯函数是什么？

纯函数是函数式编程的主要概念，它是一个接受输入并返回值而不修改作用域中其他数据的函数。

换句话说，在纯函数中，输出或返回值必须只依赖于输入值。

# Javascript 中的构造函数是什么？

构造函数是一种特殊的方法，用于在 Javascript 的类中初始化和创建对象。

我们使用带有关键字`new`的构造函数来创建一个带有新值的类似对象。

好的做法是用大写字母调用构造函数方法。

让我们看看构造函数是什么样子的，以及如何使用它。

```
function Person(name, age) {
  this.name = name;
  this.age = age;
}var man = new Person('Mark', 23);
console.log(man);
// { name: 'Mark', age: 23 }
```

在上面的代码中，我创建了一个 Person 构造函数，下面我创建了一个名为 man 的新变量，并基于 Person 构造函数创建了一个新对象。

# 什么是 Javascript 类？

自从引入了 ES6，我们可以在 Javascript 中使用类。Class 是一种函数类型，我们使用关键字`class`而不是`function`来初始化它。

除此之外，我们必须在类内部添加`constructor()`方法，每次初始化类时都会调用该方法。

在`constructor()`方法中，我们添加了类的属性。为了在现有类的基础上创建另一个类，我们使用了`extends`关键字。

在 Javascript 中使用类的一个很好的例子是 ReactJS 框架及其类组件。

# 结论

在本文中，我收集了人们在搜索引擎中问的 10 个常见的 Javascript 问题。

我已经用一种简单易懂的方式解释了它们，所以即使是初学者也可以利用这篇文章。

有些问题可以在面试时问，所以熟悉一下答案真的很有价值。

我希望您会发现这个问题列表很有用，它可以帮助您理解 Javascript 编程语言的基本概念。

![](img/31a8d5ecae4bb32b03d20e5486dea07f.png)

[Duomly —编程在线课程](https://www.duomly.com/?code=lifetime-80)

感谢您的阅读，
来自 Duomly 的安娜