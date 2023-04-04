# 您可能会犯的 Javascript 错误

> 原文：<https://blog.devgenius.io/javascript-mistakes-you-may-be-making-b316a0352c37?source=collection_archive---------6----------------------->

![](img/1535dbbb7e938f174d4d7a78eb9933a6.png)

今天我要谈谈你在 Javascript 项目中可能会犯的常见错误。

Javascript 是世界上最流行的语言之一，但是在编写代码时，由于误解或忽略我们已经知道的东西，仍然很容易出错。

# **未定义和空值**

Javascript 对于非值既有`undefined`又有`null`。然而，这两者之间有相当多的不同之处。`undefined`意味着变量可能已经被声明了，但是没有给它设置任何东西。变量也可以被显式设置为`undefined.``undefined`变量的类型，当用`typeof`操作符检查类型时，会显示类型`undefined`。不返回任何东西的函数返回`undefined`。另一方面，`null`值必须由返回`null`的函数显式设置，或者直接设置为变量。当你检查一个设置了`null`值的对象时，你会看到如果一个变量有`null`值，那么它的类型就是`object`。

出于这个原因，当您将变量值设置为非值时，尽可能坚持使用`undefined`会更容易。这减少了混淆，我们只需要检查变量的类型是否是`undefined`就可以知道它是否是`undefined`。对于`null`和`undefined`来说，这比进行两次检查要少得多。

要编写返回`undefined`的函数，您不必像这样做:

```
const f = () => {}
```

设置一个被赋予其他值的变量给`undefined`:

```
x = undefined;
```

检查属性值是否为`undefined`:

```
typeof obj.prop === 'undefined'
```

或者

```
obj.prop === undefined
```

检查变量是否为`undefined`:

```
typeof x === 'undefined'
```

一个没有被自动赋值的声明变量的值是`undefined`。

如果需要检查`null`:

```
obj.prop === null
```

或者

```
x === null
```

对于变量。因为`null`的数据类型是`object`，所以不能使用`typeof`运算符来检查`null`。

# 加法和串联

在 Javascript 中，`+`操作符既用于将两个数字相加，也用于将字符串连接在一起。因为 Javascript 是一种动态语言，所以在应用操作之前，操作数会自动转换为相同的类型。例如:

```
let x = 1 + 1;
```

那么你会得到 2，因为它们的类型相同。如您所料，`+`操作用于加法。但是，如果您有以下表达式:

```
let x = 1 + '1';
```

那么您将得到`'11'` ，因为在应用`+`操作之前，第一个操作数将被强制转换成一个字符串。`+`操作将用于连接而不是加法。当您在多个变量上使用`+`操作符时，这使得知道类型变得更加困难。在这种情况下:

```
let x = 1;  
let y = 2;  
let z = x + y;
```

你会得到 3，因为`x`和`y`都是数字。

相反，如果我们有:

```
let x = 1;  
let y = '2';  
let z = x + y;
```

然后你会得到`'12'`因为`y`是字符串，所以会用`+`运算符来代替串联。为了解决这个问题，我们应该在使用之前将所有的操作数转换成数字。这意味着我们应该将上面的代码重写为以下代码:

```
let x = 1;  
let y = '2';  
let z = Number(x) + Number(y);
```

由于我们使用工厂函数`Number`将两个操作数转换为数字，因此变量`z`将为 3。`Number`函数接受任何对象并返回一个数字，如果它能被解析成一个数字，否则返回`NaN`。另一种方法是使用`new Number(...).valueof()`功能，如下所示:

```
let x = 1;  
let y = '2';  
let z = new Number(x).valueOf() + new Number(y).valueOf();
```

因为`new Number(...)`是创建`object`类型的构造函数，所以您希望使用`valueof`函数将其转换回原始类型，以确保我们得到的是一个数字类型。一个简单的方法是:

```
let x = 1;  
let y = '2';  
let z = +x + +y;
```

单个操作数前面的`+`符号会尝试将单个操作数转换为数字，如果不能转换为数字，则转换为`NaN`。它和`Number`功能做同样的事情。您还可以将变量转换为特定类型的数字。`Number`对象有一个将字符串或对象转换成整数的`parseInt`函数和一个将字符串或对象转换成浮点数的`parseFloat`函数。`parseInt`将需要转换为数字的对象作为第一个参数。它还将基数作为可选的第二个参数，这是数学数字系统的基础。如果字符串以`0x`开头，那么基数将设置为 16。如果字符串以任何其他字符开头，那么基数将被设置为 10。

你可以这样使用它们:

```
let x = 1;  
let y = '2';  
let z = Number.parseInt(x) + Number.parseInt(y)
```

您也可以使用`parseFloat`代替`parseInt`，如下所示:

```
let x = 1;  
let y = '2';  
let z = Number.parseFloat(x) + Number.parseFloat(y)
```

# 将语句返回到多行

JavaScript 在最后关闭一个语句，所以一行代码被认为是不同的。例如，如果您有:

```
const add = (a, b) => {  
  return  
  a + b;  
}
```

如果您在运行`a + b`之前运行了结束函数执行的`return`语句，然后运行`console.log(add(1, 2));`，您将得到`undefined`。因此，`a + b`永远不会在这个功能中运行。要解决这个问题，您要么将所有的`return`语句放在一行中，要么用括号将您想要返回的内容括起来。例如:

```
const add = (a, b) => {  
  return a + b;  
}
```

如果您运行`console.log(add(1, 2));`，它将记录 3，因为您实际上是在函数中返回计算结果。你也可以写:

```
const add = (a, b) => {  
  return (  
    a + b  
  );  
}
```

这对于返回可能长于一行的表达式很方便。如果我们运行`console.log(add(1, 2));`，也会记录 3。对于箭头函数，您也可以编写:

```
const add = (a, b) => (a + b)
```

得到同样的结果。这也适用于单行箭头函数。

在 JavaScript 中，如果语句不完整，比如:

```
const power = (a) => {  
  const  
    power = 10;  
  return a ** 10;  
}
```

在函数内部，JavaScript 解释器将运行第一行和第二行来获得完整的语句。所以:

```
const  
  power = 10;
```

然而，对于像`return`语句这样的完整语句，JavaScript 解释器会将它们视为单独的行。所以:

```
return   
  a ** 10;
```

不等同于:

```
return a ** 10;
```

# 结论

即使 JavaScript 是一种友好的语言，在编写 JavaScript 代码时仍然很容易出错。当你不熟悉 JavaScript 时，很容易混淆`undefined`和`null`。由于 JavaScript 的动态类型特性，像`+`操作符这样可以做多件事的操作符很容易被转换成我们不期望的类型，并产生错误的结果。此外，如果语句本身可以是完整的，那么如果你希望两行在一起，它们就不应该写在自己的行中。

今天到此为止。

感谢您的关注，敬请关注。

编码快乐！