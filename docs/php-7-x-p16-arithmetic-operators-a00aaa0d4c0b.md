# PHP — P16:算术运算符

> 原文：<https://blog.devgenius.io/php-7-x-p16-arithmetic-operators-a00aaa0d4c0b?source=collection_archive---------29----------------------->

![](img/c2fe70080545c3076c79e85e2c8c002a.png)

算术研究数字，尤其是对这些数字的运算，如加、减、乘、除。在数学中，那些运算叫做算术运算。算术运算符是算术运算中使用的运算符。

在 PHP 中，我们有一些算术运算符。它们包括:加、减、乘、除、模和指数运算符。

让我们先浏览一下基本的。我的意思是，毕竟，这只是基本的数学。

```
<?php$a = 2 + 2; // $a = 4
$b = 5 - 3; // $b = 2
$c = $a * $b; // $b = 8
$d = $c / 2; // $d = 4?>
```

让我们更详细地看看模数运算符。它让人们困惑，但它实际上只是余数。如果你做初等除法，你说 3/2，那等于 1，余数是 1。二除以三 1 次，还剩下一次。1 的余数就是模运算符产生的结果。

模数运算符用百分比符号%表示。

```
<?php$e = $d % 2; 
// $e = 4 % 2
// $e = 0 since there's no remainder$f = 5 % 2;
// $f = 1 since there's a remainder of 1?>
```

指数运算符只是将数字提升到所提供的幂。所以如果你说 2，那就等于 2 * 2 * 2，也就是 8。PHP 中的指数运算符用双星号(**)表示。

```
<?php$g = 2 ** 3;
// $g = 2^3
// $g = 8?>
```

您不局限于在每个表达式中使用一个算术运算符。运算遵循与数学中相同的优先顺序。例如，乘法运算符在加法运算符之前执行。

让我们看看 PHP 将如何计算这个表达式。

```
<?php$h = $a + $b * $c;
// $h = $a + ($b * $c)
// $h = $a + (2 * $c)
// Remember, $c = $a * $b
// $h = $a + (2 * ($a * $b))
// $h = $a + (2 * (4 * 2))
// $h = $a + (2 * 8)
// $h = $a + 16
// $h = 4 + 16
// $h = 20?>
```

一旦我们看完了 PHP 中的所有操作符，我们将在后面的文章中详细讨论操作符优先级。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/b5f90523a9edee318dd994a3ecc738ae.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。