# JavaScript 到底是什么:作用域

> 原文：<https://blog.devgenius.io/what-is-scope-all-about-1764642a2c4c?source=collection_archive---------26----------------------->

Javascript 是关于范围的。作用域是在源代码/脚本第一次运行时创建的，每次我们编写函数时都创建一个作用域，每次我们有代码块时都创建一个作用域。

Javascript 中的作用域管理数据的可用性。每个内部级别都可以访问其外部级别。Javascript 有两个主要范围:

***全局作用域—*** *任何东西都声明在函数/代码块之外。* ***局部作用域—*** *函数/代码块内部的任何东西(不是全局作用域)。*

![](img/267e71b9e8afdafb87a1f6c929bafeed.png)

照片由[戴恩·托普金](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 全球范围

如上所述，在函数之外声明的任何东西都在全局范围内。因为作用域是管理数据可用性的东西，所以在全局作用域中定义的任何东西都可以在代码中的任何地方被访问。

```
var name = "global scope";
console.log(name);
```

每次我们运行脚本/代码片段的源代码时，都会创建全局范围。上面的代码片段有一个`name`变量，并在全局范围内声明。

```
var name = "global scope";function callMeByYourName(){
 console.log(name); // *prints "global scope"*
}
```

这里，我们有一个名为`name`的变量和一个打印该数据的函数。由于`name`在全局范围内，我们可以从内部函数访问它。

顺便说一句，它可以一直进行更深的筑巢。

```
var name = "global scope";function callMeByYourName(){

    function callMeByYourReallName(){
      console.log(name); // *prints "global scope"
    }**}*
```

> 您可以拥有任意多的嵌套函数，并且任何父作用域中的任何数据都可以被其子作用域访问。

```
var name = "global scope";function callMeByYourName(){ var name = "callMeByYourName scope"; function callMeByYourReallName(){
      console.log(name); // *prints "*callMeByYourName *scope"
    }**}*
```

注意，在上面的最后一个代码片段中，我们在全局作用域上声明了一个`name` 变量，在第二个内部作用域(`callMeByYourName()`作用域)上声明了另一个`name`变量。

由于使用了示波器机制，我们可以使用这个选项。如前所述，每个函数都创建一个新的不同的范围来管理其数据的可用性。

基本上，一旦 Javascript 到达最深的范围，它就知道需要打印出变量`name`。因为当前作用域(`callMeByYourReallName()`)没有这个变量，所以开始在父作用域中搜索这个变量。然后它到达`callMeByYourName()`范围并在那里找到它。

> 这种嵌套功能以一种方式工作。子级可以访问它们的父级范围，但反之亦然，父级不能访问它们的子级范围。

## 局部范围

每当我们创建一个函数或者一个块代码的时候**就会创建一个局部作用域。**

```
/***** Global Scope ******/
var name = "global scope";function callMyByYourName(){
 /***** Local scope *****/
 var name = "callMyByYourName scope function callMyByYourRealName(){
    /**** Local Scope ****/
    var name = "callMyByYourRealName scope";
  }}function callMe(){
 /**** Local Scope ****/
 var name = "callMe scope";
}
```

在上面的截图中，每个函数都创建了一个新的不同的作用域。每个作用域都有自己的变量`name`声明。在每个函数中，变量只在当前作用域中可见，因此每个函数使用不同的变量和不同的值。

```
function callMyByYourName(){
 var name = "callMyByYourName scope";
}console.log(name); // *prints ReferenceError: name is not defined*
```

这次我们尝试了一些不同的东西。我们在函数范围内声明了变量`name`,并在范围外调用它。

现在，你不应该对我们得到一个 *ReferenceError* 感到惊讶。如前所述，每个作用域管理自己的数据可见性，我们在`function`中声明的任何东西都存在于一个局部作用域中，并且只在那个函数中可用。

## 长话短说

Javascript 是关于范围的。你可以把它想成
——代码第一次运行时，一个全局作用域被创建
——每次函数被创建时，一个局部作用域被创建
这种机制一直“向下”(到每个嵌套的函数)继续，每次都创建一个新的作用域。
当它下降并创建作用域时，来自上层/父作用域的数据继续存在，因此可用于内部函数/作用域。(只要没有相同变量名的声明)。

> *我希望这篇文章有助于你理解什么是作用域以及它是如何工作的。范围是 Javascript 中的一个重要概念，并且在日常开发中被使用。*