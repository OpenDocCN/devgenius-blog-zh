# JS 系列# 11:JS 中的范围

> 原文：<https://blog.devgenius.io/scope-in-js-4f8ba09f5128?source=collection_archive---------13----------------------->

## 了解可见性

S cope 指定代码中可以访问特定变量的部分。定义变量的位置决定了我们可以访问该变量的位置。

![](img/b68ae3f5898f129d02dcb38d6170324c.png)

照片由 [Edryc James P. Binoya](https://unsplash.com/@ecbinoya?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

JavaScript 变量可以在 3 种类型的作用域中定义…

1.  功能范围
2.  块范围
3.  词汇范围

如果您不了解此范围，请不要担心。我们将对这 3 个示波器逐一进行更详细的讨论。

# 1.功能范围

`Function scope`规定`function`内定义的`variables`只能在`function`内访问，这些类型的变量被称为`local variable`。如果我们试图接近`function`之外的`local variable`，它会产生一个`Reference error — "variable is not defined”`。

请看一些有助于您理解`functional scope`的例子。

## 示例-1:

```
function helpMe() {
    const msg = "Hey!!! Please help me";
    console.log(msg);
}console.log(msg); // ReferenceError: msg is not definedhelpMe(); // Hey!!! Please help me
```

## 示例-2:

```
function knowMe() {
    let name = 'Alex';
    const age = 25;
    var hobby = 'Football';

    console.log(name,age,hobby);
}knowMe() // Alex 25 Footballconsole.log(name) // ReferenceError: name is not defined
console.log(age) // ReferenceError: age is not defined
console.log(hobby) // ReferenceError: hobby is not defined
```

如上例所示，分别用`let`、`const`和`var`定义的所有 3 个变量`name`、`age`和`hobby`在`function`内部可以访问，但在`function`外部不能访问，因为它们都有`functional reference`。

## 示例-3:

```
function knowMe() {
    let name = 'Alex';
    const age = 25;
    var hobby = 'Football';

    console.log(name,age,hobby);
}function updateHobby() {
    let name = 'Alex';
    const age = 25;
    var hobby = 'Cricket'; console.log(name,age,hobby)
}knowMe() // Alex 25 Football
updateHobby() // Alex 25 Cricket
```

根据`functional scope`的约定，各`function`可以有不同的`local variables`组，但不相互干扰。

正如你在上面的例子中看到的——两个函数`knowMe()`和`updateHobby()`用相同的名字定义变量，但是我们没有得到任何`error`，因为它们都是`local variables`。

# 2.块范围

如果 JS 中的`variables`在`code block`中定义，那么它们有`block scope`，并且只能在`block` 中访问，每个`block`也可以有一组独立的变量。

请看下面的例子来消化一下`block level scope`。

```
let radius = 10;
if(radius > 0) {
   const PI = 3.14;
   let area = PI * radius * radius;
   console.log(area); // 314
}console.log(radius) // 10
console.log(PI) // ReferenceError: PI is not defined
console.log(area) // ReferenceError: area is not defined
```

上面的例子解释了如何通过将变量放入`code blocks`来创建变量的`block level`范围。在上面的例子中，`PI`和`area`是在`code block`中定义的两个变量，所以只能在该块中访问，而`radius`是在`global`访问中定义的，所以可以在块外访问。

> 让我们看更多关于数据块范围的示例，这些示例向您解释了下面列出的一些要点...

1.  如何用`conditional blocks`创建`block level scope`？
2.  就作用域而言，`let`和`const`的行为与`var`有何不同？

```
**Example-1:** if(true) {
   let name = 'Alia'
   console.log(name) // Alia
}
console.log(name) // ReferenceError: name is not defined**Example-2:** if(true) {
   const name = 'Alia'
   console.log(name) // Alia
}
console.log(name) // ReferenceError: name is not defined
```

你可以从上面的例子中学到两件事…

1.  看看我们如何通过编写带有真条件 if 块的语句来创建条件块。
2.  就块而言，scope let 和 const 的工作原理是一样的，它们只能在块内被访问，不能在块外被访问。

> 让我们用 var 修改上面的例子，看看会发生什么…

```
if(true) {
   var name = 'Alia'
   console.log(name) // Alia
}
console.log(name) // Alia
```

你可以很容易地观察到，如果我们在一个`block scope`中创建一个带有`var`的变量，它不起作用。

事实上`var`只能创造`functional scope`，不能创造`block level scope`。因此，如果用`var`定义一个变量，那么可以看到各种副作用，所以对于大多数可预测的编程方法，都引入了`let`和`const`。

让我们来看一些例子，这些例子表明`var`的副作用可以被`let`或`const`克服。

```
**Example-1:**
let cars = ['VW','OODI','BMW'];for(var i = 0; i < cars.length; i++) {
    console.log(i,cars[i]);
}
console.log("Loop Ends...",i);**Output:** 0 'VW'
1 'OODI'
2 'BMW'
Loop Ends... 3
```

和上面的例子一样，为了遍历`array`，我们在一个使用`var`的循环中创建了一个变量`i`，但是在循环之外`i`仍然有效并输出 3。这通常是不可预测的。

一般的预测是，在循环之外，`i`不应该被访问。所以可以通过使用`let`关键字来改善行为。请参见下面的示例…

```
**Example-1:**
let cars = ['VW','OODI','BMW'];for(let i = 0; i < cars.length; i++) {
    console.log(i,cars[i]);
}
console.log("Loop Ends...",i);**Output:** 0 'VW'
1 'OODI'
2 'BMW'
ReferenceError: i is not defined
```

第二个副作用是用`var`关键字定义的变量可能会意外地在`loop`或其他地方被重新初始化，因为用`var`关键字重新声明变量时不会提示任何`error`。请参见下面的示例…

```
let cars = ['VW','OODI','BMW'];
var i = 10;for(var i = 0; i < cars.length; i++) {
    console.log(i,cars[i]);
} **Output:** 0 'VW'
1 'OODI'
2 'BMW'
```

但是如果我们用`let`关键字重新创建上面的例子，它会提示一个`error`，因为同一个`function scope`中的多个变量不能用`let`创建。

```
let cars = ['VW','OODI','BMW'];
let i = 10;for(let i = 0; i < cars.length; i++) {
    console.log(i,cars[i]);
} **Output:** SyntaxError: Identifier 'i' has already been declared
```

# 3.词汇范围

JS 允许函数嵌套。在这种情况下，可以在`inner function`中访问`outer function`中定义的变量，但不允许反过来。要了解事实，请看下面的例子…

```
function person() { // outer function
    let name = 'Alex';
    function showName() { // inner function
       let nickName = 'Al';
       console.log(name); // Alex
    }
    showName();
    console.log(nickName); // ReferenceError: nickname not defined
}
person() // call person function
```

上述类型的示波器称为`**lexical scope**`。

JS 的高级用例像`React JS`就是用上面的模式来创建`functional components`。请参见下面的示例…

```
function TodoList() {
    let todos = []; function addTodo() {

    } function removeTodo() {

    }
}
```

在上面的例子中，外部函数`TodoList`定义了可以在内部函数`addTodo()`和`removeTodo()`中访问的`todos`变量。

如果你喜欢这篇文章，请关注我:

**中:**https://medium.com/@maheshshittlani
**Github:**[https://github.com/maheshshittlani](https://github.com/maheshshittlani)
**LinkedIn:**[https://in.linkedin.com/in/mahesh-shittlani-638b7429](https://in.linkedin.com/in/mahesh-shittlani-638b7429)