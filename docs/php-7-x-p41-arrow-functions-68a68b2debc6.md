# PHP — P41:箭头函数

> 原文：<https://blog.devgenius.io/php-7-x-p41-arrow-functions-68a68b2debc6?source=collection_archive---------5----------------------->

![](img/8efe30714515b50c12c43526719b3295.png)

PHP 在 7.4 版本中引入了 arrow 函数语法。这只是写匿名函数的一种简化方式。PHP 中匿名函数和箭头函数的主要区别是使用了 *use* 关键字。为了能够访问匿名函数之外的作用域，你必须使用*使用*关键字。箭头函数自动访问函数外部的范围。

剖析箭头函数。

箭头功能以缩短的功能关键字 *fn()* 开头；参数可以在括号内声明。箭头(= >)紧跟在 *fn()，*之后，表示接下来发生的任何事情都将被返回。您不能在这里输入类似于 *echo* 语句的表达式，因为 arrow 函数隐式地使用了 *return* 语句。arrow 函数还可以访问全局变量 *$global_var* 。我们不必像目前为止对匿名函数所做的那样，使用关键字来实现 [*。我们可以借助于 *use* 关键字使用匿名函数来完成同样的任务。*](https://medium.com/dev-genius/php-7-x-p40-use-keyword-37d8e7df9138)

如果你读过[我在*上使用*关键词](https://medium.com/dev-genius/php-7-x-p40-use-keyword-37d8e7df9138)的文章，你就会知道:

> **使用** *获取全局变量的值当* ***函数被定义*** *和* **全局** *将获取变量的值当* ***函数被调用*** 时

因此，即使我们在定义之后，但在函数调用之前修改了全局变量，结果仍然保持不变。

在上面的例子中，第 7 行调用了 arrow 函数。arrow 函数返回 10，因为它将全局变量的值 *5 与参数的值*相加，参数的值也是 *5* 。在第 9 行，全局变量的值被更改为 *10* 。但是，当在第 11 行调用箭头函数时，它仍然显示 *10* 。如果在函数调用时检索到全局变量，值将是 15，但对于隐式或显式利用 *use* 关键字的箭头函数或匿名函数来说，情况并非如此。

现在我们已经了解了箭头函数是如何工作的，让我们看更多的例子。我们将首先创建一个闭包，然后将其转换成一个箭头函数。

闭包被分配给 *$stuff_dogs_say* ，然后在第 7 行被调用。匿名函数*呼应*出*狗说 Cays 烂*。把它转换成一个箭头函数非常简单。记住，箭头函数只有*返回*一个值；他们不能在内部使用 *echo* 语句。

我们首先创建一个变量 *$stuff_dogs_say* ，并为其分配箭头函数。箭头功能以 *fn()* 开始，后跟箭头。字符串*狗说猫吸*被隐式返回。我们只需要将字符串放在 arrow 语法的后面，arrow 函数就会立即返回它。然后，我们利用第 5 行的函数调用的 *echo* 语句来执行代码。

让我们看另一个例子。这次我们将传递一个参数给 arrow 函数。

1.  我们创建一个箭头函数。该函数用参数 *$saying* 声明。在函数调用时，参数将被连接到*狗说*字符串并被返回。
2.  PHP 在第 5 行看到了 *echo* 语句并开始执行。
3.  它调用 *$stuff_dogs_say* arrow 函数，并将 *Let Me Out* 字符串参数传递给它。
4.  PHP 进入 arrow 函数，将参数连接到 *Dogs say* 字符串并返回新字符串 *Dogs say Let Me Out* 。
5.  PHP 返回到第 5 行，而*回应*出字符串*狗说让我出去。*

这应该可以让你设置箭头函数。这实际上只是让较小的闭包变得更整洁的一种方式。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/0f01361cbb3b8fdfa9bbc7415baf2faa.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)