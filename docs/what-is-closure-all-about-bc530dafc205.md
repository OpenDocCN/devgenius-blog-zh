# JavaScript 到底是什么:闭包

> 原文：<https://blog.devgenius.io/what-is-closure-all-about-bc530dafc205?source=collection_archive---------35----------------------->

![](img/65ab833661e08be8d4b0f0896d05b93d.png)

由[萨克斯哥鲁达](https://unsplash.com/@sakethgaruda?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

当我第一次开始使用 Javascript 时，我并没有真正理解 Javascript 的主要概念。我简单地了解到 Javascript 是一种脚本语言，通过使用它，你可以创建一个动态网站，并对用户在浏览器上发生的任何事件做出反应。

老实说，有一段时间我在基础知识方面做得非常好。我曾与使用普通 Javascript(和 jQuery)的公司合作，能够毫无问题地创建很酷的动态网站或运行脚本。

当我开始从事一些更复杂和更大的项目时，一切都变了。添加简单的功能开始变得困难和耗时，调查错误变得令人沮丧，构建程序基础设施几乎是不可能的。

缺乏理解打击了我。我决定学习更多，更好地理解。我阅读了`[Javascript: the weird parts.](https://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742/ref=sr_1_2?crid=29MP0THWDR3B8&dchild=1&keywords=javascript+the+weird+parts&qid=1592709861&sprefix=javascript+the+wei%2Caps%2C243&sr=8-2)`然后我开始学习和阅读更多关于 Javascript 中的作用域以及什么是闭包。

## JavaScript 完全是关于范围的

JavaScript 中的作用域定义了你可以访问哪些变量。

JavaScript 有两个作用域——**全局**和**局部**。任何在函数外部声明的变量都属于**全局范围**，因此可以从代码中的任何地方访问。每个函数都有自己的**局部** **作用域**，在该函数中声明的任何变量都只能从该函数和任何嵌套函数中访问。

每次我们运行脚本(源代码)时，Javascript 引擎都会创建一个全局作用域，从代码中的任何地方都可以访问该作用域，并且每次我们创建函数时，基本上都是创建一个属于新创建函数的局部作用域。

在上面的代码片段中，我们有 3 个不同的范围。
1。**全局作用域** — *可以访问其嵌套作用域(* `*outerFunc()*` *和* `*innerFunc()*` *作用域)。*
2。**outer func 作用域** — *可以访问其嵌套作用域(* `*innerFunc()*` *作用域)。*
3。**inner func 作用域**

*要阅读更多关于 scope 的内容，请查看我的其他* [*帖子*](https://medium.com/@mayasavir/what-is-scope-all-about-1764642a2c4c)

## 什么是终结？

注意我们在其词法范围内调用`innerFunc()`*(`outerFunc()`*s*cope*)。*
让我们看看如果我们改变它并在其词法范围之外调用`innerFunc()`会发生什么。
**我稍微清理了一下代码，这样我们就可以继续看重要的东西了。**

*现在，我们在它的作用域(`outerFunc()`作用域)之外调用了`innerFunc()`,但它仍然可以访问声明和创建它的作用域。*

*这意味着，`innerFunc()` *在其词法范围内关闭*(捕获、记忆)变量`outerF`。
换句话说，`innerFunc()`是一个*闭包*，因为它在其词法范围内封闭了变量`outerF`。*

> *闭包是一个函数，它从定义它的地方记住变量，不管它以后在哪里执行。*

***我们来看另一个例子***

*这一次，我创建了一个 **name** 变量和一个 **callMe()** 函数。正如我们已经知道的， **callMe()** 函数在它自己的作用域中创建一个闭包，并记住它可以访问的变量——变量 **name** (由于作用域的概念)。*

*请注意，虽然 **callMe()** 函数内部的代码在 1500 毫秒后运行，但它仍然可以在脚本运行并完成后访问该变量，因为在创建 **callMe()** 函数时，它的环境包含了 **name** 变量。*

***那么预期输出是多少呢？** 如果我们将运行这个脚本，我们将看到**“Savir”**作为输出。*

***惊讶？**
闭包捕获/记忆变量**名称**而不是变量**值**。
一旦一个闭包函数**被创建**，它就存储了它可以访问的**变量**，但是**值**将等于该函数被**执行时的变量值**。*

*也就是说，一旦脚本运行，它将创建一个值为****【Maya】**的变量，然后调用函数，但该函数将在 1500 毫秒(1.5 秒)后运行。在执行该函数时，**名称变量**已经将**“Savir”**作为一个值。***

> ***闭包捕获/记忆变量**名称**而不是变量**值**。***

## ***闭包的使用***

***每次创建函数时都会创建闭包。为了使用它，我们在另一个函数内部定义一个函数，并公开它(返回它或将其传递给另一个函数)。***

*****数据隐私**
闭包通常用于给对象/变量提供数据隐私，在 Javascript 中这是主要的机制。在许多面向对象的语言中，我们可以将函数设置为私有或公共的。Javascript 没有这种能力，但是我们可以通过使用闭包来实现。***

***在上面的例子中，`increaseSalary() and salary`只在`employer()`函数中可见，从函数返回的内容(`setPromotion()`和`getSalary()`)向我们公开(在外部作用域)。***

*****穿针引线**。
闭包保留已经执行的函数状态的能力，给了我们创建 Currying 的能力。Currying 是函数式编程中的一种流行技术。它有助于局部应用。***

```
***function add(number){

 return function(anotherNumber){

   return number + anotherNumber;

 }

}let add10 = add(10);
let result = add10(10);
console.log(result); // 20***
```

***当我们第一次声明`let add10 = add(10);`时，`add10`关闭/记忆/捕获我们刚刚传递给它的参数(10)，当我们再次调用`add10`时，它会将新传递的参数添加到它已经拥有的参数中。***