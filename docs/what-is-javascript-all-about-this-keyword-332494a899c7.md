# JavaScript 到底是什么:这个关键字

> 原文：<https://blog.devgenius.io/what-is-javascript-all-about-this-keyword-332494a899c7?source=collection_archive---------37----------------------->

![](img/8294ef8ad8f8c85c91b5780aee3b7f24.png)

照片由 [Artemis Faul](https://unsplash.com/@artemisfaul?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在 JavaScript 中，`this` 关键字的行为与其他语言如 C++、Java 等稍有不同。
在这些语言中，`this` 关键字代表类的方法中当前对象的实例，而在 **JavaScript** 中，这个关键字指的是正在执行当前代码并且可以被改变的对象。

一般来说，`this`关键字指的是正在执行当前 javascript 代码的对象。因为 JavaScript 中的一切都是关于[作用域](https://medium.com/dev-genius/what-is-scope-all-about-1764642a2c4c)的，所以可以在全局和局部作用域中使用`this`关键字，其值等于该作用域中的当前对象。

> `'this’`引用当前调用函数的对象

让我们从一个例子开始

上面的代码块超级简单。我已经声明了一个名为`getName`的函数、一个名为`name`的变量和两个对象。然后，我调用函数——首先作为一个全局函数，其次作为`obj1`的一部分，然后作为`obj2.`的一部分

函数`getName`只是打印当前执行上下文的`name`属性的值(即`*this*` 对象)*。*

# `**Global Execution**`

在全局执行上下文中(任何函数之外)，`this`是指全局对象。
在 web 浏览器中，全局对象是`window`对象，而 Node.js 下运行的
脚本有一个名为`global`的对象作为它们的全局对象。
回到我们上面的代码，一旦我们调用`getName` 函数作为全局上下文的一部分，`this` 关键字引用的是全局对象，也就是`window`对象，因为我们声明了一个名为`name`的变量作为全局上下文的一部分，`this.name`将打印`Global Name`。

```
getName(); // print Global Name
```

> 每次我们将变量声明为全局上下文的一部分时，它都会在全局对象(`window` object)中创建一个新的属性。

```
var name = "Global Name";

console.log(window.name) // prints "Global Name"
```

# 本地执行

在我们的代码示例中，在调用全局函数之后，我们调用`getName`函数作为对象执行上下文的一部分。
这一次，一旦我们执行了函数，`this`将等于对象执行上下文。因此，该函数将打印分配给当前对象的`name`属性的值。

```
obj1.getName() // print "Maya"
obj2.getName() // print "Savir"
```

## 让我们看看另一个例子

这一次，我创建了一个名为`getName` 的变量，并给它分配了`object.getName`属性。`getName`作为全局上下文的一部分被调用，因此，它将打印全局`name`变量。T21:这可能会让人困惑，所以我们先讨论一下。
当我们声明`var getName = object.getName`时，我们简单地将`getName`变量赋给`object.getName`值，在我们的例子中，这只是一个将`this.name`打印到控制台的函数。所以我们也可以这样看待它，就像我们刚刚声明了一个新变量，并给它赋了一个函数，就像这样

```
var name = "Global Name";var getName = function(){
 console.log(this.name);
}getName();
```

基于前面的解释，由于我们是从全局上下文中调用`getName`函数，函数内部的`this`等于全局对象，`**getName()**` **将打印** `**Global Name**` **。**

> `**It’s important to know how, when and from where the function is called. It does not matter where the function has been declared.**`

# 隐式和显式绑定

**隐式绑定**是当`this`关键字指向函数被调用的对象时(函数被调用时句点左边是什么)。

**显式绑定**是我们显式告诉 JavaScript 引擎设置`this`指向某个值。让我们在上面的代码片段中添加两行代码。

这一次，我创建了一个变量并赋予它`object.getName`属性，然后调用这个函数值。
问题是，现在我**没有将全局变量`name`分配给全局对象`window`，因此`object2()`执行将打印`undefined`。**

对于这个场景，我们可以使用`call()`、`apply()`或`bind()`显式地告诉 JavaScript 将`this`关键字设置为一个特定的对象。

**call()和 apply()**
两者`call`和`apply`完成的任务是一样的，两者的第一个参数应该是`this`指向的东西。只有在需要将额外的参数传递给被调用的函数时，这种差异才显而易见。**如果你不知道将要传递的参数的数量，或者如果它们已经在一个数组或类似数组的对象中(比如 arguments 对象来转发你自己的参数)，使用 apply** 。**否则使用 call** ，因为不需要将参数包装在数组中。

**bind()** `bind`用于创建一个永久绑定到`this`值的新函数。在下面的例子中，我们创建了一个新的函数，它的`this`永久绑定到了`getName` ，并将`getName` 重新分配给这个新的永久绑定的函数。

> 每次我们执行`getName()`函数时，我们会将它绑定到`person`对象，因此，‘`this’`将指向`person`对象。

# 新绑定

当使用`new`关键字创建一个函数对象的实例时，我们将函数用作构造函数。
当一个函数作为构造函数被调用时，`this`指向创建的新对象。

当使用`new`关键字时，
*一个全新的空对象被创建。
*新的空对象链接到那个函数的原型属性
* ***这个*** 关键字绑定到同一个新的空对象，用于那个函数调用的执行上下文

在上面的代码片段中，`Person`函数被调用时前面带有`*new*`关键字。它创建一个新对象，然后这个新对象被链接到函数`Person`的原型链，之后创建的新对象被绑定到`Person`函数的`*this*`对象。

想知道为什么第 10 行显示“未定义”吗？
在第 10 行，已经创建了一个类型为`Person`的新对象，并且已经执行了`Person`功能。此时，`Person`函数被调用，当它到达第 4 行时，它打印到控制台`name`和`this.lastName`。
问题是，在`Person`函数中并没有真正的`this.lastName`，但是它有一个名为`lastName`的变量——这不是一回事！
`name`变量来自全局声明(第 7 行)。

当`maya.name`被打印到控制台(第 11 行)时，js 引擎知道`maya`是一个类型为`Person`的对象，并且`Person`有一个`this.name`属性。由于`this`指向`Person`，我们在控制台上看到`Maya`。
*如果我尝试控制`maya.lastName`，我将再次看到`undefined`，因为这个. lastName(或 maya.lastName)与`Person`函数中的变量`lastName`不同。

# 箭头函数绑定

箭头函数不绑定它们自己的`this`，相反，它们从父作用域继承一个。

```
**const** getName = () => {   
 console.log(**this**); // *this* refers to the global object
}; getName();
```

预期结果将与正常函数相同，`window or global`对象。结果一样，原因不一样。对于普通函数，作用域默认绑定到全局作用域， **arrows 函数**没有自己的`this`，但是它们从父作用域继承，在本例中是全局作用域。

```
**const** person = {   
 myName: null,   
 getName: **function** () {     
  **this**.myName = () => { console.log(**this**) };   
 } 
};person.getName();
person.myName();
**const** myName = person.myName; 
myName();
```

当我们调用`person.getName()`时，我们用方法`getName`内部的箭头函数初始化`person.myName()`，因此它将继承它的作用域。**是** [**闭包**](https://medium.com/dev-genius/what-is-closure-all-about-bc530dafc205) 的完美案例。

## 严格模式与非严格模式

在**严格模式的情况下，**全局这将默认为`undefined`而在**非严格模式的情况下** `this`将默认为全局对象。

```
function getName(){
 'use strict';
 console.log(this);
}getName() // prints *undefined* (instead of the global object)
```

**“this”关键字绑定的优先级** *首先检查该函数是否用 ***新*** 关键字调用。
*其次检查函数是否用 *call()* 或 *apply()* 方法调用，即显式绑定。
*第三，检查函数是否通过上下文对象调用(隐式绑定)。
*默认全局对象(在严格模式下未定义)。