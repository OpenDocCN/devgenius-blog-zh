# PHP — P40:使用关键字

> 原文：<https://blog.devgenius.io/php-7-x-p40-use-keyword-37d8e7df9138?source=collection_archive---------1----------------------->

![](img/b1cf7d4895786b31396d980574aba181.png)

在前几节教程中，我们已经学习了函数。每次需要访问函数外部的数据时，都可以声明一个参数并将一个参数传递给函数。但是，如何在不使用参数的情况下访问函数外部的变量呢？我知道我们还没有看到范围，但是我将尝试用几句话来解释它。

作用域是程序的某个部分可以访问的变量和方法的可见性。例如，假设我们在函数内部声明了一个变量(正则或闭包)。其他函数将无法访问该变量。该变量对创建它的函数具有局部作用域。变量可以有全局范围，可以在任何地方访问；那些变量是在函数之外定义的。然而，在 PHP 中，在函数内部调用具有全局范围的变量是不允许的，因为函数会寻找局部变量。在使用全局变量之前，必须在函数内部将其声明为 *global* 。我们最终将在本文中讨论这一点。别担心，用几个例子很快就会明白了。

您可以使用 *use* 和 *global* 概念从**匿名**函数内部访问全局变量，但是要知道 *use* 在函数被定义时获取全局变量的值，而 *global* 将在函数被调用时获取变量的值。 *use* 关键字只能**用于闭包，不能用于常规函数。**

在我们看一个例子之前，让我们看一下使用 *use* 关键字的闭包的结构。*如果你需要匿名函数(闭包)的帮助，请看* [*我的文章主题*](https://medium.com/@dinocajic/php-7-x-p39-anonymous-functions-75f6f99fa571) *。*

*use* 关键字紧接在 *function()* 声明之后，左花括号之前；您将在括号内传递全局变量。让我们来看一个例子。

1.  PHP 将值字符串 *Dino* 赋给变量 *$name* 。
2.  闭包 *$hello* 被创建。匿名函数在函数定义期间导入全局 *$name* 变量。一旦函数被调用，它将*回显*变量 *$name* 的值。
3.  在第 9 行，PHP *回显*变量 *$name* 中的值。目前， *$name* 正在存储字符串 *Dino* ，因此 *Dino* 显示在屏幕上。
4.  接下来，在第 11 行，一个新值 *Harrison* 被赋给变量 *$name* 。
5.  在第 13 行，PHP *回显*变量 *$name* 中的值。目前， *$name* 正在存储字符串*哈里森*，因此*哈里森*显示在屏幕上。
6.  最后，调用闭包$ *hello()* 。
7.  PHP 返回到第 5 行，开始执行闭包体内的内容。调用 *echo* 语句，显示 *Dino* 。

我们在第 11 行把 *$name* 的值改成了 *Harrison* ，为什么会显示 *Dino* ？记住我之前说的:

> **使用**在**函数被定义时获取全局变量的值**而**全局**将在**函数被调用**时获取变量的值

当 PHP 读取程序时，它在第 5 行定义了函数，并复制了全局变量的值，以便可以使用它。后来，在第 14 行，闭包被调用。已经执行了定义部分，并且值已经固化在闭包内部。这就是为什么你得到的值是*迪诺*而不是*哈里森*。

当然，您可以将全局变量作为参数传递，但是如果您需要使用 *use* 函数，这就是您应该做的。如上所述，两者都有好处。

让我们看看最后一个例子，它包含了*使用*关键字、*全局*关键字和*参数*概念。

1.  PHP 首先将值 *Dino* 和 *Harrison* 分别赋给变量 *$name1* 和 *$name2* 。
2.  PHP 定义了匿名函数。匿名函数是用一个参数 *$greeting* 声明的。该参数将在函数调用过程中传递。匿名函数利用 *use* 关键字来复制 *$name1* 全局变量的值。然后，它利用*全局*关键字来访问 *$name2* 全局变量的值。最后，它将*回显*出 *$greeting* 参数和两个全局变量 *$name1* 和 *$name2。*
3.  在第 12 行调用闭包，并传递 *Hello there* 字符串。PHP 进入闭包并显示:*你好，迪诺和哈里森。*
4.  它退出闭包并在第 14 行继续，将 *$name1* 的值从 *Dino* 更改为 *Amy。*
5.  在第 15 行，存储在 *$name2* 中的值从 *Harrison* 更改为 *Steve。*
6.  第 17 行再次调用闭包。字符串*在那里*作为参数被传递。
7.  PHP 进入闭包并显示:*嗨，迪诺和史蒂夫*。

即使两个全局变量都改变了，就匿名函数而言，只有 *$name2* 被更新了，因为 *use* 在定义**函数时得到全局变量的值，而**和 *global* 在调用**函数时得到变量的值。**

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dino cajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/f04e0f12613630f4b56f898f64c2173f.png)

Dino Cajic 现任[LSBio(life BioSciences，Inc.)](https://www.lsbio.com/) 、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛峰生物技术](https://everestbiotech.com/)、 [Nordic MUbio](https://www.nordicmubio.com/) 和[exa lpha](https://www.exalpha.com/)IT 总监。他还担任了 [MyAutoSystem](https://myautosystem.com/) 的首席执行官。他有超过十年的软件工程经验。他拥有计算机科学学士学位和生物学辅修学位。他的背景包括创建企业级电子商务应用程序，进行基于研究的软件开发，以及通过写作促进知识的传播。

您可以在[领英](https://www.linkedin.com/in/dinocajic/)上与他联系，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡奇(以及其他数千名媒体作家)的每一个故事。您的会员费直接支持 Dino Cajic 和您阅读的其他作者。您还可以完全了解 Medium 上的每个故事。*](https://dinocajic.medium.com/membership)