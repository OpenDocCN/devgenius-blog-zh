# 用于测试自动化的 Javascript)

> 原文：<https://blog.devgenius.io/javascript-for-test-automation-e5-e3803ccd7fd8?source=collection_archive---------15----------------------->

# 功能

你经常需要在一个程序中多次执行一些代码。可能需要跑两三次，也可能需要跑几百万次。无论如何，你都不想继续写那些代码。你有什么选择？循环是一种可能性，尽管它们并不总是有益的。相反，大多数编程语言都提供了一个过程特性，允许您分离代码并将其作为独立单元运行。这些方法在 JavaScript 中被称为函数。

## 使用函数

在使用函数之前，必须先使用 reserved keyword 函数声明该函数。在单词 function 后面写下函数名，后跟一对括号(())。您希望与函数相关联的代码(函数体)插入在右括号后的花括号()之间。例如，这里有一个名为`say`的函数:

```
function say() {
  console.log("Hi!");
}
```

创建一个包含以下代码的文件名`say.js`:

```
//say.js
console.log("hello");
console.log("hi");
console.log("how do you do");
console.log("Quite all right");
```

请注意我们是如何多次复制 console.log 的。我们不希望每次需要输出时都调用它，而是希望有一个在需要记录时可以调用的代码。用以下代码更新`say.js`:

```
function say(words) {
  console.log(words);
}say("hello");
say("hi");
say("how do you do");
say("Quite all right");
```

乍一看，这个软件似乎很荒谬。它并没有减少代码量，而是增加了相当多的代码。此外，上述方法没有提供 console.log 所没有的任何功能。然而，这里有一个优点。我们抽象了显示文本的逻辑，使我们的软件更加通用。如果我们以后需要改变显示特定文本的方式，我们可以在一个地方完成:say 函数。我们不需要编辑任何其他代码来获得修改后的行为。

通过输入函数名和特定的可选值(称为参数)来调用函数。say.js 中的函数定义在函数名后面包含(单词)。这个符号表示当我们调用函数时，我们应该只给(传递)一个参数。参数允许您将数据从函数范围之外传输到函数，以便函数可以访问它。如果函数定义不需要访问外部数据，则不需要参数。

函数定义中括号内的名字称为参数。自变量代表参数的值。JavaScript 中的变量名包括函数名和参数名。局部变量，尤其是参数，只在函数体内局部定义。

当我们使用 say 函数时，我们必须给 word 参数一个值，一个参数。例如，在第 5 行，我们为函数提供了值“hello ”,它用这个值来初始化单词。该值可以以函数认为合适的任何方式使用。值得注意的是，参数的范围是函数定义；比如说，它不能在户外使用。

```
function say(words) {
  console.log(words + "!");
}say("hello"); // => hello!
say("hi"); // => hi!
say("how are you"); // => how are you!
say("I'm fine"); // => I'm fine!
```

要查看结果，请运行以下代码。在修改了一行代码之后，我们添加了一个！到每一行。你可以看到现在的功能有多强大。

## 返回值

一个常见的应用是执行一个操作，然后将结果返回到调用位置供后续使用。这就是我们对**返回** **值**和**返回**语句所做的事情。

JavaScript 函数调用都返回值。默认情况下，这个值是未定义的，大多数 JavaScript 函数的隐式返回值是。当使用 return 语句时，可以从函数中返回指定的值。这是一个明确的返回值。函数外的**隐式和显式返回值**没有区别。尽管如此，重要的是要记住，所有函数在引发异常之前都会返回一些东西，即使它们不执行 return 语句。

让我们创建一个`add`函数，返回两个数字的和:

```
function add(a, b) {
  return a + b;
}add(2, 3); // returns 5
```

执行该程序时不会记录任何内容，因为 add 不调用 console.log 或任何其他输出方法。但是，该函数确实返回值:5。当 JavaScript 找到 return 语句时，它对表达式求值，结束函数，并将表达式的值返回到调用它的位置。为了确保这一点:让我们将返回值保存在一个变量中，并将其记录到终端。

```
let twoAndThree = add(2, 3);
console.log(twoAndThree); // => 5
```

JavaScript 中的 return 语句用于向调用函数的代码返回值:调用者。如果没有指定值，则返回 undefined。在这两种情况下，return 语句都会终止函数，并将控制权转移给调用者。谓词是总是返回布尔值，即真或假的函数。

## 默认参数

当定义一个函数时，你可能希望对它进行组织，以便它可以在没有参数的情况下被调用。让我们改变一下，如果调用者没有提供参数，就使用默认值。

```
function say(words = "hello") {
  console.log(words + "!");
}say("Howdy"); // => Howdy!
say();        // => hello!/* You'll note that say(), when called without any parameters, prints "hello!" to the console. We may call our function without parameters since we've specified a default value for words. */
```

## 嵌套函数

可以在任何地方创建函数，包括在另一个函数内部:

```
function foo() {
  function bar() {
    console.log("BAR");
  }bar(); // => BAR
  bar(); // => BAR
}foo();
bar(); // ReferenceError: bar is not defined/* The bar function is nestled within the foo function in this case. Every time the outer function runs, such nested functions are produced and removed. (This has a little impact on performance.) They are also private functions since we cannot access a nested function from outside the function where it is declared. */
```

## 功能和范围

JavaScript 中的变量根据可访问的位置分为两类:**全局**变量和**局部**变量。局部变量限于一个函数或块，而**全局**变量在整个程序中都是可用的。用于声明变量的关键字及其声明位置决定了变量是全局变量还是局部变量。目前，我们将忽略 var，转而关注 let 和 const。let 或 const 变量的位置会影响它是全局变量还是局部变量。

全局变量

顾名思义，全局变量具有全局范围，这意味着它们在程序中的任何地方都是可用的。您可以随时阅读并重新分配它们。函数或块中定义的任何变量都被视为局部变量；其他任何东西都被认为是全局变量。

让我们看一个例子。将下面的代码添加到名为`greeting.js`的文件中并运行它。

```
//greeting.jslet greetingMessage = "Good Morning!";
console.log(greetingMessage);
```

结果是预料之中的；上面印着“早上好！”在控制台上。因为 greetingMessage 变量不是函数声明的一部分，所以它是一个全局变量。因为它是在世界一级声明的，所以它的范围是全球性的。让我们通过添加一个 greetingMessage:函数来显著修改我们的软件。

```
let greetingMessage = "Good Morning!";function greetPeople() {
  console.log(greetingMessage);
}greetPeople();
```

该软件生成的结果与之前相同。但是，这里使用的是函数内部的 greetingMessage。我们可以这样做，因为 greetingMessage 是一个全局变量，可以在任何地方使用。即使在函数内部，我们也可以重新分配全局变量。

```
let greetingMessage = "Good Morning!";function greetPeople() {
  console.log(greetingMessage);
}function changeGreetingMessage(newMessage) {
  greetingMessage = newMessage;
}changeGreetingMessage("Good Evening");
greetPeople(); // => 'Good Evening'
```

我们已经修改过了。我们在程序中添加了 GreetingMessage 方法，该方法将 greetingMessage 重新分配给作为输入传递的新字符串。第 11 行调用该方法并向其传递文本“Good Evening”，该文本成为全局 greetingMessage 的新值。在某些情况下，如应用程序范围的设置，全局变量可能是有利的。然而，大多数开发人员不鼓励使用它们，因为它们经常会导致问题。一般来说，你应该尽可能缩小你的变量范围；较小的变量范围降低了外部范围误用变量的可能性。

局部变量

顾名思义，JavaScript 中的局部变量有一个局部范围，这意味着它们不能在声明它们的函数之外被访问。像全局变量一样，局部变量的位置定义了它的作用域。让我们看一个如何使用局部变量的例子。

```
function greetPeople() {
  let greetingMessage = "Good Morning!";
  console.log(greetingMessage);
}greetPeople();
```

上述代码在功能上等同于上一节中的第一个示例。但是，它不使用任何全局变量。在内部，greetPeople 函数声明 greetingMessage。它在方法内部可用，但在外部使用它会导致 ReferenceError。

```
function greetPeople() {
  let greetingMessage = "Good Morning!";
  console.log(greetingMessage);
}greetPeople();
console.log(greetingMessage); // raises ReferenceError
```

要观察错误消息，请使用 node 运行初始应用程序。这个主题提出了一个有趣的问题:如果我们已经在第 2 行硬编码了我们的欢迎消息，我们如何指示 greetPeople 生成一个新的欢迎消息？当然，函数参数是答案:

```
function greetPeople(greetingMessage) {
  console.log(greetingMessage);
}greetPeople("Good Morning!");
```

greetingMessage 参数的功能类似于局部变量。毕竟，它是一个局部变量。唯一的区别是我们使用函数的参数来初始化它。在函数中，参数有一个局部范围。这就把我们带到了局部变量的另一个重要特征。局部变量有一个短暂的生存期；当对应于它们作用域的函数终止时，它们就消失了。当我们调用函数时，我们开始一个新的作用域。如果该作用域的代码定义了一个新变量，该变量就属于该作用域。当该范围内的最终代码完成时，该范围内的局部变量将被移除。当我们在 JavaScript 中调用一个函数时，这个过程会重复。

## 函数与方法

到目前为止，我们所有的函数调用都使用了 functionName(obj)语法。我们调用一个函数，用圆括号把它括起来，并给它提供零个或多个参数。您可能希望使用类似 toUpperCase 的函数调用将字符串转换为全大写字符(string)。但是，您必须使用一种称为方法调用的独立语法。当您在变量名或变量值前加上句点(。)到函数调用，例如“xyzzy”。toUpperCase()。这样的函数被称为方法。

## 变异呼叫者

一个方法有时会永久地影响调用它的对象:它改变了调用者。为了与非突变方法进行比较，请考虑以下几点:

```
let name = "Pete Hanson";
console.log(name.toUpperCase()); // => 'PETE HANSON'
console.log(name);               // => 'Pete Hanson'
```

toUpperCase 字符串方法是一种非破坏性(非突变)操作。它保留以前的字符串值:“皮特·汉森”非可变方法，如 toUpperCase()，经常传递一个新的值或对象，而不改变调用者。

有些方法会永久改变对象。

```
let oddNumbers = [1, 3, 5, 7, 9];
oddNumbers.pop();
console.log(oddNumbers); // => [1, 3, 5, 7]
```

pop()函数删除数组的最后一个成员，但这是永久性的:修改是不可逆的。Pop 就地修改数组，而 String toUpperCase 函数提供的新值是原始字符串的修改版本。换句话说，它改变了调用者(数组)。

我们也可以讨论函数是否改变它们的参数。让我们编写一个函数来演示这个概念:

```
function changeFirstElement(array) {
  array[0] = 9;
}let oneToFive = [1, 2, 3, 4, 5];
changeFirstElement(oneToFive);
console.log(oneToFive); // => [9, 2, 3, 4, 5]
```

此代码中使用了[index]语法来修改发送给 changeFirstElement 方法的数组的第一个条目。当函数完成时，我们可以观察到原始数组已经改变。并不是所有的函数都这样；我们的问候功能不会改变提供给它们的数据。创建一个向数组中添加新元素并返回新数组的函数，以查看非破坏性数组函数的示例:

```
function addToArray(array) {
  return array.concat(10);
}let oneToFive = [1, 2, 3, 4, 5];
console.log(addToArray(oneToFive)); // => [1, 2, 3, 4, 5, 10]
console.log(oneToFive);             // => [1, 2, 3, 4, 5]
```

concat 函数返回一个新数组，该数组包含原始数组的副本和参数提供的额外元素。因为 concat 制作了原始数组的副本，然后对副本进行了变异，所以原始数组保持不变，如第 7 行所示。在处理数组和对象时，变异是一个问题，但在处理整数、文本和布尔值等原始值时，变异就不是问题了。原始值是不可改变的。它们的值是不可变的:对不可变对象的操作总是返回新值。可变值操作(数组和对象)可能会也可能不会产生新值，可能会也可能不会更改数据。

## 功能组成

让我们看一些简单的函数组合的例子。首先，让我们定义`add`和`subtract`函数并调用它们:

```
function add(a, b) {
  return a + b;
}function subtract(a, b) {
  return a - b;
}let sum = add(20, 45);
console.log(sum); // => 65let difference = subtract(80, 10);
console.log(difference); // => 70
```

add 和 remove 都包含两个参数，并返回对我们认为是数值的内容执行算术运算的结果。这工作完美。然而，JavaScript 允许我们在一个称为函数组合的过程中，将一个函数调用作为另一个函数的参数。换句话说，我们可以将 add(20，45)和 subtract(80，10)作为另一个函数的输入:

```
console.log(add(20, 45)); // => 65
console.log(subtract(80, 10)); // => 70
```

JavaScript 中函数组合的典型例子是将函数调用的返回值传递给 console.log。当您将函数调用结果传递给执行更复杂任务的函数时，生活会变得更加有趣:

```
function times(num1, num2) {
  return num1 * num2;
}console.log(times(add(20, 45), subtract(80, 10))); // => 4550
// 4550 == ((20 + 45) * (80 - 10))
```

这里，我们向 times 函数提供 add(20，45)和 subtract(80，10)的结果，times 的结果被传递到 console.log！它与以下详细代码具有相同的效果:

```
let sum = add(20, 45);
let difference = subtract(80, 10);
let result = times(sum, difference);
console.log(result);
```

让我们看一个更复杂的例子:

```
add(subtract(80, 10), times(subtract(20, 6), add(30, 5))); // => 560
```

让我们来分析一下这段代码的作用:

1.  首先，我们将两个参数传递给`add` : `subtract(80, 10)`和`times(subtract(20, 6), add(30, 5))`。
2.  第一个参数是`subtract`函数调用，返回`70`。
3.  第二个参数是`times`函数调用，此外还有两个参数:`subtract(20, 6)`和`add(30, 5)`。

*   `subtract(20, 6)`返回`14`
*   `add(30, 5)`返回`35`
*   使用返回值，整个函数调用变成`times(14, 35)`
*   `times`调用的总价值是`490`

4.使用步骤 2 和 3 的返回值，我们得到返回`560`的`add(70, 490)`。

## 定义函数的三种方法

```
function functionName(zeroOrMoreArguments...) {
  // function body
}
```

像这样的函数定义在 JavaScript 中被称为函数声明。在声明函数之前调用它的能力是函数声明的一个显著特征。让我们看一个在声明函数之前调用它的例子:

```
greetPeople();function greetPeople() {
  console.log("Good Morning!");
}
```

将上面的代码放在一个. js 文件中，并使用 node 执行它。你会看到它运行没有任何问题。让我们看看另一种定义函数的方法，叫做函数表达式。

```
let greetPeople = function () {
  console.log("Good Morning!");
};greetPeople();
```

这可能看起来不寻常，但它是 JavaScript，您会经常看到它。它的大部分似乎是一个常规的函数声明。然而，因为我们把它存储到一个变量中，所以它是一个函数表达式。函数表达式在一个关键方面不同于函数声明:在函数表达式存在于你的程序之前，你不能调用它。在=符号之后，我们的示例定义了一个名为 greetPeople 的变量，并将它赋给函数表达式。因为 JavaScript 函数是**一级函数**，所以我们可以这样做。一级函数的主要优点是它们可以像其他值一样被处理。**实际上，所有的 JavaScript 函数都是对象**。因此，它们可能被赋给变量，作为参数发送给其他函数，并从函数调用中返回。

任何不以单词 function 开头的函数定义都称为函数表达式。即使将看似函数声明的内容括在括号中，也会产生一个函数表达式:

```
(function greetPeople() { // This is a function expression, not a declaration
  console.log("Good Morning!");
});
```

函数表达式的另一个典型例子出现在高阶函数中:

```
function makeGreeter(name) {
  return function greeter() {
    console.log(`Hello ${name}`);
  };
}
/* However, look closely at the definition of the greeter function. That is not a function declaration -- it's a function expression. */
```

箭头函数是 JavaScript 中的第三种函数。箭头函数在语法上与函数声明和表达式有很大不同。让我们来看一个:

```
let greetPeople = () => console.log("Good Morning!");
greetPeople();
```

这是迄今为止我们所看到的功能的重大转变。箭头函数的语法类似于函数表达式的语法。然而，区别不仅仅是句法上的。现在，考虑一下箭头函数的一个有趣的方面:隐式返回。首先，我们将改变前一节的 add 函数到 arrow 函数的语法:

```
let add = (a, b) => a + b;
```

那就短很多了！请注意缺少 return 语句。在箭头函数中，当且仅当函数体包含单个表达式时，我们可以省略它(表达式可以有子表达式，但整个表达式必须计算为单个值)。假设它有两个或更多的短语或断言。如果需要一个值，必须显式返回它，并且还必须使用花括号:

```
let add = (a, b) => a + b;
let getNumber = (text) => {
  let input = prompt(text);
  return Number(input);
};let number1 = getNumber("Enter a number: ");
let number2 = getNumber("Enter another number: ");
console.log(add(number1, number2));/* On line #2, we define an arrow function that requires one parameter. The parentheses around the parameter name are optional in this case and are often omitted. */
```

## 调用堆栈

调用堆栈的概念，或者更通俗地说，堆栈，是所有程序员都必须掌握的函数的一个重要特性。调用堆栈允许 JavaScript 跟踪哪些函数当前正在运行，以及当函数返回时应该在哪里重新开始执行。在这一点上，它的功能类似于一摞书:如果你有一摞书，你可以在最上面增加一本新书或者去掉最上面的那本书。类似地，调用堆栈将关于当前函数的信息放在堆栈的顶部，然后在函数返回时移除它。让我们假设我们有下面的代码:

```
function first() {
  console.log("first function");
}function second() {
  first();
  console.log("second function");
}second();=>first function
=>second function
```