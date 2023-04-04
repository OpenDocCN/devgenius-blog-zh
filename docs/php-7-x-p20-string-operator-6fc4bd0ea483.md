# PHP — P20:字符串操作符

> 原文：<https://blog.devgenius.io/php-7-x-p20-string-operator-6fc4bd0ea483?source=collection_archive---------29----------------------->

![](img/87c319647b2014c3a3944b8eb90b632e.png)

字符串运算符，也称为连接运算符，允许我们将两个字符串连接在一起。出于各种原因，需要连接字符串，例如注入变量，或者甚至拆分长字符串，以便在 IDE 中更具可读性。

我们来看看下面这句话(字符串):“我已经厌倦了创建操作员教程。”我们可以回显它或者把它赋给一个变量。我们也可以使用连接操作符来分割字符串，字符串的数量与字符的数量一样多。

```
<?phpecho "I am sick and tired of creating operator tutorials.";
echo "I am sick and " . "tired" . " of creating operator tutorials";echo "2024 can't come soon enough. " . 
     "As soon as it does, I am " .
     "buying a GT-R.";?>
```

当你拆分一个字符串的时候，有一点需要注意:记住空格。这些东西很快就会丢失。

```
<?phpecho "Hello" . "there"; // Hellothere
echo "Hello " . "there"; // Hello there?>
```

我们也可以将变量连接成字符串。

```
<?php$year = 2020;
$make = "Nissan";
$model = "GT-R";// Outputs: 2020 Nissan GT-R
echo $year . ' ' . $make . " " . $model;?>
```

串联运算符可用于用单引号和双引号括起来的字符串。字符串中的连接运算符只是一个句点(一个标点符号)。串联运算符只能放在两个字符串之间。

字符串也可以与数值连接，包括浮点数。

```
<?php// Outputs: I am 32 years old
echo "I am " . 32 . " years old;// Outputs: It is $42.99
echo "It is $" . 42.99;?>
```

您可能在其他面向对象编程语言中见过点运算符，比如 Java。这些语言中的点运算符允许您访问对象的方法和属性。大多数语言使用+运算符连接两个字符串。在 PHP 中，我们使用箭头操作符，-->，来访问对象的方法和属性，并且使用连接操作符(。)来连接两个字符串。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/966d4204362feba905a80f91bd1b483c.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。