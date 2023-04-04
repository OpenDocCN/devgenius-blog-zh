# 函数式编程

> 原文：<https://blog.devgenius.io/functional-programming-ef4870aa8dea?source=collection_archive---------36----------------------->

![](img/f1d03fc848ef408afecfebc12da4bfc2.png)

作为一个开始学习 Ruby，后来又学习 Python 的开发人员，我首先从面向对象编程开始，并简要地了解了函数式编程。

当我第一次开始学习 React 上的钩子时，我不得不学习这两种编程范例之间的区别，虽然有关于哪一种更好的讨论，但这不是本文的目标。

## 首先，什么是面向对象编程

![](img/f0776d2bad441ce5f56e5ded00594c4e.png)

类可以被看作是盒子，里面有方法和属性

也称为 OOP，基于“对象”，可以包含数据(如属性)和方法(或函数)。对象拥有的一个东西是自我的概念(或“这个”)，在这里它可以称自己为改变它的属性。

使用 OOP 的一些语言: [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) ， [C++](https://en.wikipedia.org/wiki/C%2B%2B) ， [C#](https://en.wikipedia.org/wiki/C_Sharp_(programming_language)) ， [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) ， [R](https://en.wikipedia.org/wiki/R_(programming_language)) ， [PHP](https://en.wikipedia.org/wiki/PHP) ， [JavaScript](https://en.wikipedia.org/wiki/JavaScript) ， [Ruby](https://en.wikipedia.org/wiki/Ruby_(programming_language)) ， [Perl](https://en.wikipedia.org/wiki/Perl) ， [Object Pascal](https://en.wikipedia.org/wiki/Object_Pascal) ， [Objective-C](https://en.wikipedia.org/wiki/Objective-C) ，[Dart](https://en.wikipedia.org/wiki/Dart_(programming_language))

OOP 的主要缺点之一是重用代码的困难，尤其是类内部的函数。

## 现在，什么是函数式编程

它通过应用和编写函数来编程。它们基于单一责任的原则，其中函数将有返回值。

函数的概念来自数学(更具体地说是λ演算)，数学家将他们的计算“分解”成更小、更容易理解的块，从而产生一个值；这种抽象使得计算大量信息而不迷失方向成为可能。

虽然相似之处到此为止，但原则仍然是相同的，在函数式编程中，你会有一个方法来做一件事，并且每次被调用时总是以相同的方式运行，这种方法的最大优点是，你有一个针对错误的安全措施，因为你知道事情应该做什么，并且它更容易调试，因为你可以一步一步地进行，直到你找到行为不端的代码。

函数式编程将看起来像一个巨大的拼图，所有的部分将产生一个完整的图像，而不会失去它们仍然是一个整体的事实。

## 他们是一等公民，这意味着什么

在函数式编程中，函数被视为一等公民，这意味着它们可以被赋予名称、作为参数传递以及从其他函数返回。

所以一个函数可以调用另一个函数，或者像其他数据类型一样返回一个函数。

## 使用

虽然函数式编程不如面向对象编程流行，但许多语言仍在使用它: [Common Lisp](https://en.wikipedia.org/wiki/Common_Lisp) 、 [Scheme](https://en.wikipedia.org/wiki/Scheme_(programming_language)) 、 [Clojure](https://en.wikipedia.org/wiki/Clojure) 、 [Wolfram 语言](https://en.wikipedia.org/wiki/Wolfram_Language)、[球拍](https://en.wikipedia.org/wiki/Racket_(programming_language))、 [Erlang](https://en.wikipedia.org/wiki/Erlang_(programming_language)) 、 [OCaml](https://en.wikipedia.org/wiki/OCaml) 、 [Haskell](https://en.wikipedia.org/wiki/Haskell_(programming_language)) 和 [F#](https://en.wikipedia.org/wiki/F_Sharp_(programming_language)) ，其他语言支持函数式编程: [C++11](https://en.wikipedia.org/wiki/C%2B%2B11)