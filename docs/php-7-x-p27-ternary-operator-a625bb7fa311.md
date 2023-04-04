# PHP — P27:三元运算符

> 原文：<https://blog.devgenius.io/php-7-x-p27-ternary-operator-a625bb7fa311?source=collection_archive---------8----------------------->

![](img/07ccac633b3d5698e0cec284e9f43da7.png)

三元运算符是一种快速表达 if/else 语句的方式。三元运算符遵循以下语法:

```
( boolean expression ) ? if_true : if_false;
```

如果布尔表达式的计算结果为真，则显示“if_true”结果，否则显示“if_false”结果。

简单回顾一下，普通的 if/else 语法如下所示:

```
<?php$expression = true;if ( $expression ) {
  // do statement if true
} else {
  // do statement if false
}?>
```

这是多余的，尤其是当你只是重复陈述的时候。您将需要编写两个不同的 echo 语句:一个用于如果为真则为 T0 的部分，另一个用于如果为假则为 T2 的部分。三元运算符本身可以压缩到一个 echo 语句中。

```
echo ( boolean expression ) ? if_true : if_false;
```

让我们看一个例子。

```
<?php$money = 2000;echo ( $money >= 2000 ) ? "Time to spend" : "Need more money";?>
```

由于变量 *$money* 包含 2000，表达式 *$money ≥ 2000* 的计算结果为真，因此“花费时间”显示在屏幕上。如果存储在 *$money* 变量中的值小于 2000，您会在屏幕上看到“需要更多的钱”。

当你需要在做出一些决定后给一个变量赋值时，三元运算符也很有用。

```
<?php$money = 2000;$isBuyingAMountainBike = ( $money >= 2000 ) ? true : false;?>
```

在上面的语句中，PHP:

1.  将 *2000* 赋值给 *$money* 变量。
2.  前进到 *$isBuyingAMountainBike* 变量。它看到它包含一个三元运算符。
3.  PHP 对表达式求值， *$money ≥ 2000。*表达式返回*真*。
4.  由于表达式返回了 *true，* PHP 进入三元表达式的“if_true”部分，并看到它包含值 *true* 。
5.  值 *true* 被分配给 *$isBuyingAMountainBike* 变量。

对于上面的三元表达式，我们可以很容易地放置 Yes/No 或 1/0 值，而不是 true/false。

让我们看一个三元运算符真正开始发光的例子。假设我们有一个变量*$ employeeorcuster*。我们正在写一封电子邮件，根据变量的值，我们想要打印一个正式的问候，或者一个更轻松的问候。我们将把整个消息存储在变量 *$msg* 中。如果我们不熟悉三元运算符，我们可能会使用 if/else 语句来生成我们的 *$msg* 值。

添加了 if/else 语句，以便它可以做出决定。它试图判断是否会将“dude”或“sir/madam”添加到 *$msg* 值中。多亏了三元运算符，我们可以用一条语句获得相同的结果。

三元运算符不应被滥用；它应该只用于简单的 if/else 决策。如果您有嵌套表达式，您应该尝试借助逻辑运算符(如&&运算符)将嵌套布尔表达式转换为单个表达式，或者简单地使用 if/else 结构。

三元运算符在您的编程生涯中会经常用到，所以请习惯它。你会喜欢的。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/c303f2ac397763b2524197da959d5de8.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)