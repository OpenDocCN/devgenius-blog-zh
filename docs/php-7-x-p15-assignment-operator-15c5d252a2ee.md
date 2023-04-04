# PHP — P15:赋值运算符

> 原文：<https://blog.devgenius.io/php-7-x-p15-assignment-operator-15c5d252a2ee?source=collection_archive---------24----------------------->

![](img/31ebda92d1055a7b5c0190626b696281.png)

赋值运算符(=)将值或表达式赋给变量。是的，它不同于等式比较运算符(==)。这是初学程序员最常犯的错误之一:在需要比较运算符时使用赋值运算符。

赋值运算符需要以下语法:

> $variable =值(或表达式)

赋值操作将右边的表达式赋给左边的变量。

```
<?php// Assign a value to a variable
$best_php_tutorial_youtuber = "Dino Cajic"; // of course// Assign an expression to a variable
$best_expression = 5 + 7;/** Another expression
 *  These two strings are concatenated
 *  and are then assigned to the 
 *  variable on the left.
 */
$another_expression = "Hello " . "there";?>
```

这就是事情开始变得有点奇怪的地方。您可以将一个值赋给一个变量，然后将该变量赋给另一个变量。您可以继续运行任意多次，但是代码可读性会受到影响。

```
<?php$x = ($y = 2) + 22;?>
```

让我们看看 PHP 如何处理上面的赋值运算符示例:

1.  PHP 首先将值 2 赋给变量$y。
2.  形成了一个新的表达式:$y + 22。
3.  该表达式被求值。用值 2 代替$y，然后加到 22: **2 + 22 = 24** 。
4.  新值 24 被赋给$x。变量$x 现在包含值 24。

我们现在可以同时使用$x 和$y 变量。

```
<?phpecho $x; // prints 24
echo $y; // prints 2?>
```

出于可读性的目的，我建议在给变量赋值时尽可能明确。下面的代码比前一个例子中的代码更容易阅读。

```
<?php$y = 2;
$x = $y + 22;?>
```

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/7531e77d88958b2ff85920a1fd681a4b.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。