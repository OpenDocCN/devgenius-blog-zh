# PHP — P30: Do While 循环

> 原文：<https://blog.devgenius.io/php-7-x-p30-do-while-loop-e70777a17ee3?source=collection_archive---------6----------------------->

![](img/24c1b9ac8b0d6b5bf5bdf84a37259a29.png)

如果被求值的表达式**从不**为真，则常规的 [*while* 循环](https://medium.com/@dinocajic/php-7-x-p29-while-loops-2f178ae3d747)将跳过循环体。但是如果我们需要主体的内容至少被执行一次呢？这就是 *do-while* 循环为我们做的事情。

看看上面的代码， *while* 循环永远不会执行，因为表达式总是假的。PHP 会跳过它。在本例中，我们希望该语句至少执行一次。我们希望 PHP 显示“我将至少执行一次”,不管表达式的计算结果是否为真。

概括一下， [while 循环](https://medium.com/@dinocajic/php-7-x-p29-while-loops-2f178ae3d747)的处理方式如下:

1.  PHP 测试 *while* 循环中的表达式。
2.  如果为 true，则执行循环体内的代码。回到 1 并重复。
3.  如果为 false，则退出循环，并在循环的右花括号后继续执行代码。

*do-while* 循环翻转 1 和 2。它首先执行循环体内的内容，然后测试条件。即使条件为假，循环体内部的代码也已经执行过一次。如果条件为真，它将再次执行循环体内部的内容，并不断重复该过程，直到循环表达式的计算结果为假。

*do-while* 循环结构如下:

```
do {
  code to execute
} while ( expression );
```

让我们把前面例子中的代码从一个 *while* 循环改为一个 *do-while* 循环。

这一次，循环体内部的代码在 PHP 退出循环之前执行一次。让我们看另一个例子。我们将创建一个 *while* 和一个 *do-while* 循环来完成相同的任务:显示从 0 到 9 的数字。

在上面的 while 循环中，PHP 检查以确保 *$i* 小于 10。在第一次迭代中，0 小于 9。PHP 进入循环体，输出 0 到屏幕。[后递增运算符(++)](https://medium.com/@dinocajic/php-7-x-p18-increment-and-decrement-operators-98422028d1a5) 将 *$i* 从 0 递增到 1，PHP 再次测试条件。它一直这样做，直到显示 9。一旦显示 9， *$i* 递增到 10，表达式“ *$i* 小于 10”失败。

在 *do-while* 循环中，PHP 执行循环体内的代码。值 0 显示在屏幕上，并且 *$i* 从 0 增加到 1。表达式测试:0 <是 10 吗？既然是这样，循环体的内容就会再次执行。迭代继续，直到显示 9。变量 *$i* 从 9 增加到 10，并且条件失败。循环结束。

如你所见，在这种情况下，我们可以使用 *while* 或 *do-while* 循环，因为它产生相同的结果。唯一不同的情况是， *$i* 最初被设置为大于 9 的值。由于 *while* 循环在执行代码之前进行测试，因此永远不会到达代码。 *do-while* 循环在测试前执行代码，因此它至少会显示一次结果。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/4d4bc67bdf23b961817943a604e67c0f.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体刊物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。