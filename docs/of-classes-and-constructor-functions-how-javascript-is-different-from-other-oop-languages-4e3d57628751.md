# 类和构造函数:JavaScript 与其他 OOP 语言有何不同

> 原文：<https://blog.devgenius.io/of-classes-and-constructor-functions-how-javascript-is-different-from-other-oop-languages-4e3d57628751?source=collection_archive---------11----------------------->

![](img/f1aa16f6e968b3a6c8d454bae70a0eac.png)

由[海洋运球创造](https://dribbble.com/shots/6998335-Robot-animation#shot-description)

[有人提出了一个关于 JavaScript 中函数和构造函数的区别的问题。这个问题源于 JavaScript 臭名昭著的名声，即*不是*真正的面向对象语言。](https://dev.to/gnio/eli5-functions-vs-class-constructor-in-javascript-nki)

虽然这是真的(我们将在后面讨论)，但与传统的 OOP 语言(如 C++、Java 或 Python)相比，流行的文献大多解释了这一点。这不仅没有帮助，而且对那些不熟悉这些语言的人来说也很困惑。

所以在这篇文章中，我将试图弄清楚 JavaScript 类与传统 OOP 类的区别。我将使用 Python 作为这些语言的代表，因为它易于理解，并且相对接近 JavaScript。

# 传统面向对象语言

一个`class`通常被定义为对象的蓝图。它有两个实际用途:

*   抽象:哪些信息是相关的？这无关紧要。
*   封装:如何显示或隐藏相关或不相关的内容？

在它的核心，一个`class`有两种类型的属性:`members`和`methods`。这些属性定义了存储在`class`中的数据以及`class`可以对这些数据做什么操作。

为了利用一个`class`，我们通过一个叫做实例化的过程来创建这个类的`instances`。每个`instance`得到`class`的`members`和`methods`的*隔离*副本。让我们看看这在 Python 中是如何工作的:

在本例中，`person_a`和`person_b`是`Person`的`instances`。他们每个人都有自己的`first_name`和`last_name`成员，以及自己的`print_full_name`方法。

现在在 Python 中，你只需直接调用`class`就可以执行实例化(就像我们如何创建`person_a`和`person_b`)。然而，传统上并不总是这样。例如，在 C++和 Java 中，您需要添加关键字`new`，以便能够实例化`class`。我认为这是混乱的开始。

# Java Script 语言

在 JavaScript 中，我们用关键字`new`调用了一个叫做**的构造函数**。这些构造函数是类的 JavaScript 模拟。现在，虽然看起来这和我们提到的其他语言是一样的，但是每当我们使用这些构造函数时，JavaScript 的行为就不同了。看，每当我们使用`new`关键字来执行一个构造函数时，我们实际上是告诉 JavaScript 正常运行该函数，但是在幕后有两个额外的步骤:

1.  在函数的开头创建了一个隐式对象，我们可以用`this`引用它。
2.  结果实例在其自己的原型中有一个构造函数的原型属性的副本。

现在不要担心细节，因为我们以后会谈到这些。让我们先看看如何在没有任何奇特构造函数的情况下创建一个 JavaScript 对象:

这工作得非常好！为什么不到此为止，结束这一切呢？

好吧，残酷而诚实的事实是，我们*可以*。通过这种方式简单地创建对象，我们可以完成很多事情。但是这样做，我们就忽略了 JavaScript 作为一种基于原型的语言的全部意义。这是它区别于传统 OOP 语言的独特之处(不一定更好也不一定更差)。

现在让我们看看如何用另一种方式实现它。当您阅读下面的代码片段时，请记住当使用`new`调用构造函数时，在幕后发生的额外两个步骤。

这就是奇迹发生的地方。如您所见，当我们创建`Person`类时，我们将定义成员的地方(`firstName`和`lastName`)和定义方法的地方(`fullName`)分开。`firstName`和`lastName`就在你期望的地方:在构造函数定义里面。但是有趣的部分是我们定义`fullName`的地方，那是在构造函数的`prototype`里。

为什么这很重要？这很重要，因为每当我们通过 `**new**` **关键字创建一个新的**`**instance**`**`**Person**`**构造函数时，一个引用了** `**prototype**` **属性的构造函数就会被添加到** `**__proto__**` **对象的属性中。再读一遍。之后，再读一遍。确保你拿到了。****

**与传统的 OOP 语言相反，方法不会被复制到构造函数(或类)的每个实例中。当我们调用`personA.fullName()`时，JavaScript 不是在实例本身中查找方法，而是查看`personA`的`__proto__`属性，然后*沿着*向上爬，直到找到`fullName`。由于我们在`Person.prototype`中定义了`fullName`，并且`Person.prototype`与`personA.__proto__`相同，所以当我们调用`personA.fullName()`时，我们调用的是一个不存在于实例中，而是存在于构造函数本身中的方法！这提供了性能优势，因为这些方法只需定义一次(在构造函数的原型上)。也就是说:**

**这意味着我们在`Person.prototype`上定义的任何东西都可以用于`Person`的所有实例。实际上，我们可以做一些奇怪的事情(从传统 OOP 的角度来看),比如:**

**所以你有它。总结一下:**

*   **每当用`new`调用构造函数时，它们都在后台做两件事:创建一个可以用`this`引用的隐式对象，并将每个实例的`__proto__`属性赋给构造函数的`prototype`属性**
*   **当在实例上调用一个函数时，`__proto__`属性会一直攀升，直到找到对被调用函数的引用。这意味着每个实例都没有对方法的引用，但是所有实例都共享在构造函数上定义的同一个方法。**
*   **在传统的 OOP 中，每个方法的所有实例都有一个副本。没有原型的概念。**

# **ES6“类”是什么**

**ES6“类”并没有真正引入我们传统上所知道的类。这使得编写构造函数变得更加容易，因为你不必为每个你想在实例间共享的方法编写`prototype`。ES6 类语法是一种更简单的方法，可以将一个构造函数的所有成员和方法都存储在一个地方，同时还可以抽象出`prototype`和它带来的所有混乱。**

**作为一个例子，我们可以用下面的方式编写`Person`构造函数:**

**您可以看到它看起来非常类似于我们的 python 示例(但是您和我都知道它们是不一样的！).尝试创建`Person`的实例，并亲自查看`prototype`属性！😉**

***原发布于*[*https://www . Adrian perea . dev*](https://www.adrianperea.dev/of-classes-and-constructor-functions-how-javascript-is-different-from-other-oop-languages/)*。***