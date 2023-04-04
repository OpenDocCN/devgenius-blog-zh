# JS 系列#12:一级函数和高阶函数

> 原文：<https://blog.devgenius.io/js-series-12-first-class-functions-higher-order-functions-b69537d4612?source=collection_archive---------9----------------------->

## 现代写作方式的功能

![](img/8fd8dcce656570269207db0ddc540f0c.png)

照片由 [MJ Tangonan](https://unsplash.com/@mjtangonan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

到目前为止，我们一直遵循传统的写作风格，如下例所示…

```
function square(x) {
    return x * x
}Square(5) // 25
```

还有另一种书写或定义`function`的方式叫做`function expression`。请参见下面的示例…

```
const square = function(x) {
    return x * x
}// square(5)
```

在上面的例子中，我们已经移除了`function name`并创建了一个`anonymous function`，它被分配给一个名为`square`的变量。

在一个变量中，我们可以分配一个`Number`、`String`、 `array`和`objects`，但是函数如何分配给一个变量呢？所以答案是，`functions`在 JavaScript 中都是`objects`。

让我们看更多的例子来理解`function expression` …

```
function(a,b) {
    return a + b
}
```

在上面的代码片段中，创建了一个没有名字的`function`，甚至它也没有在`variable`中赋值。所以这种类型的`function`被称为`anonymous function`。
所以避免创作`anonymous function`。

但是如果将`anonymous function`赋给一个变量，它就变成了`function expression`，可以像`function`一样使用。请参见下面的示例…

```
const add = function(a,b) {
    return a + b;
}add(5,6); // 11
```

但是，如果我们给一个`function`命名，同时又给它分配一个`variable`会发生什么呢？请参见下面的代码片段…

```
const product = function multiply(a,b) {
    return a * b;
}product(4,5) // 20
multiply(3,4) // ReferenceError: multiply is not defined
```

一旦在`variable`中赋值了`function`，它就会被`variable`引用，而`function name`没有任何意义。所以把`function name`写在`function expression`里是个馊主意。

# 一级功能

当一种编程语言中的函数可以被视为类似于`variable`时，该编程语言被称为具有`First-class Function`。例如，如果一个函数可以存储在一个`variable`中，作为一个`argument`在一个`function`中传递，而作为`returned`由另一个`function`传递。

## 将函数存储到变量中

```
const add = function(a,b) {
    return a + b;
}const sub = function(a,b) {
    return a - b; 
}const mult = function(a,b) {
    return a * b;
}
const divide = function(a,b) {
    return a / b;
}
```

在上面的例子中，我们创建了 4 个函数，并将它们存储到变量中，如`add`、`sub`、`mult`、`divide`。

对于更复杂的例子，这些变量可以存储到数组和对象中。

## 示例|存储在数组中的函数

```
const ops = [add,sub,mult,divide]for(let i = 0; i < ops.length; i++) {
    console.log(ops[i](20,10));
}**Output:** 30
10
200
2
```

## 示例|存储在对象中的函数

```
const obj = {
  add: add,
  sub: sub,
  mult: mult,
  divide: divide
}obj.add(10,5)
obj.sub(10,5)
obj.mult(10,5)
obj.divide(10,5)**Output:** 15
5
50
2
```

# 高阶函数

操作其他功能的`function`。他们可以:

1.  接受其他函数作为参数
2.  返回一个函数

## Example | Function 接受其他函数作为参数

```
const wishMe = function(action) {
   action()
}const sayGoodMorning = function () {
   console.log('Good morning');
}const sayGoodEvening = function() {
   console.log('Good Evening');
} wishMe(sayGoodMorning); // Good Morning
wishMe(sayGoodEvening); // Good Evening
```

## 示例|函数返回其他函数

```
function multiplyBy(num) {
   return function(x) {
        return num * x;
   }
} const double = multiplyBy(2);
const triple = multiplyBy(3);console.log(double(10)) // 20
console.log(triple(7)) // 21
```

> **示例:**创建一个范围函数工厂，通过返回一个函数**来检查任意范围。**

```
function makeRange(min,max) {
    return function(x) {
       return x >= min && x <= max;
    }
}const isChild = makeRange(0,18)
const isEighties = makeRange(1981,1989)
const isPleasentWeather = makeRange(15,25)//Check the output with above created function
console.log(isChild(22)) // false
console.log(isEighties(1986)) // true
console.log(isPleasentWeather(18)) // true
```

# 回调函数

C allback 是作为`argument`传递给另一个函数并由`outer function`调用的函数。

实际上，我们已经在本文中练习了`callback`的例子，但是`callback`在 JavaScript 中是如此的`buzzword`，所以最好再修改一次。

## 修订后的回拨示例

```
const wishMe = function(action) {
   action()
}const sayGoodMorning = function () {
   console.log('Good morning');
}const sayGoodEvening = function() {
   console.log('Good Evening');
}wishMe(sayGoodMorning); // Good Morning
wishMe(sayGoodEvening); // Good Evening
```

在上面的例子中，`sayGoodMorning()`和`sayGoodEvening()`是回调函数，它们被传入`wishMe()`函数并在其中被调用。

如果你有一点使用`JavaScript`的经验，你应该已经体验过常用的`callback`功能。下面讨论一些常见的`callback`函数的例子。

```
**Example-1:**
setTimeOut(function() {
   alert('Hey!!! Time up')}
,1000);**Example-2:** const btn = document.querySelector('button');
btn.addEventListener('click',function(evt){
    console.log('Button Clicked');
});
```

# 提升

`Hoisting`是一个过程，其中所有的声明无论是`variable declaration`还是`function declaration`，都自动移动到程序的顶部。但这只发生在`var`关键字上。

所以，不要浪费一分一秒的时间，快速进入一些例子…

```
var car = 'BMW';
console.log(car); // BMW
```

在上面的代码片段中，创建并打印了一个`variable`,但是如果我们交换上面两个语句的顺序会发生什么呢？

```
console.log(car); // undefined
var car = 'BMW';
```

让我们用 Let 试试上面的代码片段，了解一下有什么区别。

```
console.log(car); // ReferenceError: car is not defined
let car = 'BMW';
```

正如你所看到的，如果一个`variable`没有被定义但是被使用了，它应该产生一个`error`而不是`“variable is not defined”`。

但是在`var`的情况下，结果是未定义的，因为`hoisting`发生了，所有的变量声明都被移到了顶部，看起来像下面的代码片段…

```
var car;
console.log(car); //undefined
car = 'BMW';
```

当您定义`functions`时，同样的事情也会发生。所有的函数声明都自动移动到顶部。

```
greet() // Hi!!!function greet() {
    console.log('Hi!!!');
}
```

普通函数是`hoisted`，但函数表达式不是`hoisted`。

```
welcome(); // TypeError: welcome is not a function
var welcome = function(){
    console.log('Welcome');
}
```

如果你喜欢这篇文章，请关注我:

**中:**[https://medium.com/@maheshshittlani](https://medium.com/@maheshshittlani)
**Github:**[https://github.com/maheshshittlani](https://github.com/maheshshittlani)
**LinkedIn:**[https://in.linkedin.com/in/mahesh-shittlani-638b7429](https://in.linkedin.com/in/mahesh-shittlani-638b7429)