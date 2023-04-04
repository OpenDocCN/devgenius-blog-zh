# PHP — P39:匿名函数

> 原文：<https://blog.devgenius.io/php-7-x-p39-anonymous-functions-75f6f99fa571?source=collection_archive---------6----------------------->

![](img/ea55349490208ad0ddd01219b1d9613d.png)

匿名函数，或者说闭包，是那些似乎给人们的生活带来很多压力的函数。让我们简化这个过程。匿名函数是没有名字的函数。

我们来看一个正则函数。一个**常规**函数以*函数*关键字开始，后跟函数名；参数用括号括起来，函数体用花括号括起来。

**匿名**函数删除函数名，并将函数赋给一个变量。

让我们创建一个实际的例子。我们将展示一个标准函数和一个匿名函数声明。让我们从常规函数开始。函数名为 *stuff_dogs_say()* 。它只会在被叫了之后发出“狗说猫逊”的回声。我们已经在 [*用户定义函数*](https://medium.com/dev-genius/php-7-x-p35-user-defined-functions-c6e23a7309c5) 一文中研究了如何调用常规函数。

现在，让我们将常规函数转换成匿名函数。

我们创建一个名为 *$stuff_dogs_say* 的变量，并为其分配匿名函数。因为我们要给变量赋值，所以我们需要像往常一样用分号结束赋值语句。为了清楚起见，该函数被分隔成多行。由于 *$stuff_dogs_say* 包含了匿名函数，我们可以通过在变量名本身后面加上一对括号来调用这个函数: *$stuff_dogs_say()* 。

我们可以声明参数并将参数传递给匿名函数，就像我们处理常规函数一样。

我们用参数 *$saying* 声明匿名函数，并将其赋给 *$stuff_dogs_say_two* 变量。匿名函数只是将传递给它的参数回显出来。在第 7 行的函数调用过程中，参数*猫臭*被传递给 *$stuff_dogs_say_two* 函数。

让我们看一些有趣的东西。我们知道我们可以从函数内部返回任何数据类型。这意味着我们也可以返回函数；我知道，很惊讶。我们将创建一个返回匿名函数的常规函数。让我们来看一个例子，并从头到尾看一遍。

1.  我们首先创建一个名为 *stuff_cats_say()* 的常规函数。常规函数只是返回一个匿名函数。匿名函数包含一个 *echo* 语句:这里没有什么革命性的东西。我们知道在这里我们可以返回任何东西，所以我们本来可以返回一个整数(即*返回 3；)*或一个字符串(即*返回“Dino”；*)。我们这次只是返回一个完整的函数(即 *return function() { echo“狗吸”；};*)
2.  我们将常规函数 *stuff_cats_say()* 赋给第 9 行的 *$cats* 变量。记住 PHP 是如何工作的。它从赋值操作符的右边开始。它评估 *stuff_cats_say()* 函数。那个函数只是返回一个匿名函数。然后这个匿名函数被分配给变量 *$cats* 。会跟说: *$cats = function() { echo“狗吸”；};*
3.  现在我们知道了变量 *$cats* 包含了匿名函数，我们知道如何调用这个匿名函数:通过在变量后面附加一对括号。
4.  *$cats()* 函数调用将调用匿名函数。匿名函数是做什么的？它只是*呼应*出来的*狗吸。*

为什么要使用匿名函数而不是常规函数？有时你需要访问一个函数范围之外的变量，由于某种原因，这个变量不能作为参数传递。您可以在匿名函数中使用“use”关键字来访问该变量的范围。我们将在下一篇文章中讨论“使用”关键字。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/0be89e76469a3ac5a4e333b17f372600.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)