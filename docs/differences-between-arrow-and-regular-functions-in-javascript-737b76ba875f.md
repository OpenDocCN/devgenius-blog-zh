# Javascript 中箭头函数和常规函数的区别

> 原文：<https://blog.devgenius.io/differences-between-arrow-and-regular-functions-in-javascript-737b76ba875f?source=collection_archive---------6----------------------->

![](img/25989fe2cc29a303eb028ba6a00bf803.png)

弗洛里安·奥利佛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

可以用多种方式定义 JavaScript 函数。

第一种通常的方法是使用`function`关键字:

```
// Function declaration
function greet(who) {
    return `Hello, ${who}!`;
}// Function expression
const greet = function(who) {
    return `Hello, ${who}`;
}
```

我要作为常规函数引用的函数声明和函数表达式。

从 ES2015 开始，第二种方法是箭头函数语法:

```
const greet = (who) => {
    return `Hello, ${who}!`;
}
```

虽然常规语法和箭头语法都定义函数，但是什么时候你会选择一个而不是另一个呢？这个问题问得好。

在这篇文章中，我将展示两者之间的主要区别，这样你就可以根据自己的需要选择正确的语法。

# 目录

*   这个值
*   构造器
*   参数对象
*   隐性回报
*   方法
*   摘要

# 1.这个值

## 正则函数

在常规 JavaScript 函数内部，`this`值(也称为执行上下文)是动态的。

动态上下文意味着`this`的值取决于函数是如何被调用的。在 JavaScript 中，有 4 种方法可以调用常规函数。

在一个简单的调用过程中，`this`的值等于全局对象(或者如果函数在严格模式下运行，则等于`undefined`):

```
function myFunction() {
    console.log(this);
}// Simple invocation
myFunction(); // logs global object (window)
```

在方法调用期间，`this`的值是拥有该方法的对象:

```
const myObject = {
    method() {
        console.log(this);
    }
}// Method invocation
myObject.method(); // logs myObject
```

在使用`myFunc.call(thisVal, arg1, ..., argN)`或`myFunc.apply(thisVal, [arg1, ..., argN])`的间接调用中，`this`的值等于第一个参数:

```
function myFunction() {
    console.log(this);
}const myContext = { value: 'A' };myFunction.call(myContext);  // logs { value: 'A' }
myFunction.apply(myContext); // logs { value: 'A' }
```

在构造函数调用期间，使用关键字`new`等于新创建的实例:

```
function MyFunction() {
    console.log(this);
}new MyFunction(); // logs an instance of MyFunction
```

## 箭头功能

箭头函数内部的`this`行为与常规函数的`this`行为有很大不同。arrow 函数没有定义自己的执行上下文。

无论在何处或以何种方式执行，箭头函数内的`this`值总是等于外部函数的`this`值。换句话说，arrow 函数在词汇上解析`this`。

在下面的例子中，`myMethod()`是`callback()`箭头函数的外部函数:

```
const myObject = {
    myMethod(items) {
        console.log(this); // logs myObject
        const callback = () => {
            console.log(this); // logs myObject
        };
        items.forEach(callback);
    }
};myObject.myMethod([1, 2, 3]);
```

`this`箭头函数`callback()`内的值等于外部函数`myMethod()`的`this`。

`this`词法解析是 arrow 函数的一大特点。当在方法中使用回调时，你肯定 arrow 函数没有定义自己的`this`:没有更多的`const self = this`或`callback.bind(this)`变通方法。

与常规函数相反，使用`myArrowFunc.call(thisVal)`或`myArrowFunc.apply(thisVal)`对 arrow 函数的间接调用不会改变`this`的值:上下文值总是按词法解析的。

# 2.构造器

## 正则函数

正如上一节所看到的，正则函数可以很容易地构造对象。

例如，`new Car()`函数创建一辆汽车的实例:

```
function Car(color) {
    this.color = color;
}const redCar = new Car('red');redCar instanceof Car; // => true
```

`Car`是常规函数。当用`new`关键字`new Car('red')`调用时，创建`Car`类型的新实例。

## 箭头功能

`this`的词法解析的结果是箭头函数不能用作构造函数。

如果您试图调用以`new`关键字为前缀的箭头函数，JavaScrip 会抛出一个错误:

```
const Car = (color) => {
    this.color = color;
};const redCar = new Car('red'); 
// TypeError: Car is not a constructor
```

调用`new Car('red')`，其中`Car`是一个箭头函数，抛出`TypeError: Car is not a constructor`。

# 3.参数对象

## 正则函数

在常规函数体内，`arguments`是一个特殊的类似数组的对象，包含调用函数所用的参数列表。

让我们用两个参数调用`myFunction()`函数:

```
function myFunction() {
    console.log(arguments);
}myFunction('a', 'b'); // logs { 0: 'a', 1: 'b', length: 2 }
```

在`myFunction()`主体内部，`arguments`是一个类似数组的对象，包含调用参数:`'a'`和`'b'`。

## 箭头功能

另一方面，在箭头函数中没有定义特殊关键字。

再次说明(与`this`值相同)，`arguments`对象被词法解析:arrow 函数从外部函数访问`arguments`。

让我们尝试访问箭头函数内部的`arguments`:

```
function myRegularFunction() {
    const myArrowFunction = () => {
        console.log(arguments);
    }
    myArrowFunction('c', 'd');
}myRegularFunction('a', 'b'); // logs { 0: 'a', 1: 'b', length: 2 }
```

箭头函数`myArrowFunction()`被参数`'c'`、`'d'`调用。然而，在其主体内部，`arguments`对象等于`myRegularFunction()`调用的参数:`'a'`，`'b'`。

如果你想访问 arrow 函数的直接参数，那么你可以使用[rest 参数](https://dmitripavlutin.com/javascript-function-parameters/#5-rest-parameters)特性:

```
function myRegularFunction() {
    const myArrowFunction = (...args) => {
        console.log(args);
    }
    myArrowFunction('c', 'd');
}myRegularFunction('a', 'b'); // logs ['c', 'd']
```

`...args` rest 参数收集 arrow 函数的执行参数:`['c', 'd']`。

# 4.隐性回报

## 正则函数

`return expression`语句返回函数的结果:

```
function myFunction() {
    return 42;
}myFunction(); // => 42
```

如果缺少`return`语句，或者 return 语句后没有表达式，正则函数隐式返回`undefined`:

```
function myEmptyFunction() {
    42;
}function myEmptyFunction2() {
    42;
    return;
}myEmptyFunction();  // => undefined
myEmptyFunction2(); // => undefined
```

## 箭头功能

可以像从常规函数中返回值一样从 arrow 函数中返回值，但有一个有用的例外。

如果 arrow 函数包含一个表达式，并且您省略了函数的花括号，那么表达式将隐式返回。这些是内嵌箭头函数。

```
const increment = (num) => num + 1;
increment(41); // => 42
```

`increment()`箭头只包含一个表达式:`num + 1`。该表达式由 arrow 函数隐式返回，不使用`return`关键字。

# 5.方法

## 正则函数

正则函数是在类上定义方法的常用方式。

在下面的类`Hero`中，使用常规函数定义了方法`logName()`:

```
class Hero {
    constructor(heroName) {
        this.heroName = heroName;
    }

    logName() {
        console.log(this.heroName);
    }
}const batman = new Hero('Batman');
```

通常，常规函数作为方法是可行的。

有时你需要提供一个回调方法，比如给`setTimeout()`或者一个事件监听器。在这种情况下，您可能会遇到访问`this`值的困难。

例如，让我们使用 use `logName()`方法作为对`setTimeout()`的回调:

```
setTimeout(batman.logName, 1000);
// after 1 second logs "undefined"
```

1 秒钟后，`undefined`被记录到控制台。`setTimeout()`执行对`logName`(其中`this`是全局对象)的简单调用。这就是方法与对象分离的时候。

让我们手动将`this`值绑定到正确的上下文:

```
setTimeout(batman.logName.bind(batman), 1000);
// after 1 second logs "Batman"
```

`batman.logName.bind(batman)`将`this`值绑定到`batman`实例。现在你确定这个方法没有丢失上下文。

手动绑定`this`需要样板代码，尤其是当你有很多方法的时候。有一种更好的方法:箭头充当类字段。

## 箭头功能

多亏了[类字段提议](https://github.com/tc39/proposal-class-fields)(此时在阶段 3)你可以在类中使用箭头函数作为方法。

现在，与常规函数相比，使用箭头定义的方法将`this`在词汇上绑定到类实例。

让我们将箭头函数用作字段:

```
class Hero {
    constructor(heroName) {
        this.heroName = heroName;
    } logName = () => {
        console.log(this.heroName);
    }
}const batman = new Hero('Batman');
```

现在您可以使用`batman.logName`作为回调，而无需手动绑定`this`。`logName()`方法中`this`的值总是类实例:

```
setTimeout(batman.logName, 1000);
// after 1 second logs "Batman"
```

# 摘要

理解常规函数和箭头函数之间的差异有助于为特定需求选择正确的语法。

`this`常规函数中的值是动态的，取决于调用。但是箭头函数内部的`this`是词汇绑定的，等于外部函数的`this`。

`arguments`常规函数内的对象包含参数列表。相反，arrow 函数没有定义`arguments`(但是您可以使用 rest 参数`...args`轻松访问 arrow 函数参数)。

如果 arrow 函数有一个表达式，那么即使不使用`return`关键字，表达式也会被隐式返回。

最后但同样重要的是，您可以在类内部使用 arrow 函数语法来定义方法。粗箭头方法将`this`值绑定到类实例。

无论如何，粗箭头方法被调用，`this`总是等于类实例，这在方法被用作回调时很有用。

为了理解 JavaScript 中所有类型的函数，我推荐查看 6 种声明 JavaScript 函数的方法。

**想要连接？** 本文由马来西亚 [Arkmind](https://arkmind.com.my) 技术负责人韩胜撰写。他对软件设计/架构相关的东西、计算机视觉以及边缘设备充满热情。他开发了几个基于人工智能的网络/移动应用程序来帮助客户解决现实世界的问题。请随意通过他的 [Github 个人资料](https://github.com/hansheng0512)来了解他。