# PHP — P22:速记员

> 原文：<https://blog.devgenius.io/php-7-x-p22-shorthand-operators-bdef003cd52d?source=collection_archive---------18----------------------->

![](img/1d180cd898c08131fd6ca4941ebfab71.png)

速记员太棒了！在那里，我说了。速记操作符将右边的表达式与赋值操作符组合在一起。出现在赋值运算符左侧的变量也需要出现在右侧，以便您能够使用速记符号。

让我们看看下面的代码:

```
<?php$x = 1;
$x = $x + 1;echo $x;?>
```

1.  PHP 将整数值 1 赋给变量$x。
2.  在第二个语句中，$x + 1 被求值。$x 包含值 1，因此结果将是 1 + 1，等于 2。
3.  值 2 被分配给$x。

如果您回显$x，将显示 2。

由于变量$x 出现在赋值运算符的两边，我们可以使用速记运算符来缩短表达式$x = $x + 1。您只需从右侧移除$x，并将+运算符移到=运算符前面:$x += 1。

```
<?php// Long approach
$x = $x + 1;// Can be shortened to
$x += 1;?>
```

速记操作不限于整数；您可以使用串联简写运算符来组合字符串。

```
<?php// Before
$msg = "Hey";
$msg = $msg . " there";// After
$msg  = "Hey";
$msg .= " there";?>
```

我们可以将同样的逻辑应用于减法、乘法，甚至是模运算。

```
<?php$x = 0;// Same as $x = $x + 4;
// $x = 0 + 4; 
// $x = 4
$x += 4;// Same as $x = $x - 2;
// $x = 4 - 2;
// $x = 2;
$x -= 2;// Same as $x = $x * 2;
// $x = 2 * 2;
// $x = 4;
$x *= 2;// Same as $x = $x % 2;
// $x = 4 % 2;
// $x = 0;
$x %= 2;?>
```

速记操作符在整个程序设计中经常使用；它们如此频繁，以至于你很少能在野外看到它们。有些操作被如此频繁地使用，以至于产生了更短的操作符。我说的是[递增和递减运算符](https://medium.com/dev-genius/php-7-x-p18-increment-and-decrement-operators-98422028d1a5)，我们最近已经讨论过了。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dino cajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/71629e2c4ea2cf1ab3d1461cca38bea9.png)

Dino Cajic 现任[LSBio(life BioSciences，Inc.)](https://www.lsbio.com/) 、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛峰生物技术](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 总监。他还是 [MyAutoSystem](https://myautosystem.com/) 的首席执行官。他有超过十年的软件工程经验。他拥有计算机科学学士学位和生物学辅修学位。他的背景包括创建企业级电子商务应用程序，进行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。