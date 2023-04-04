# PHP — P24: If 语句

> 原文：<https://blog.devgenius.io/php-7-x-p24-if-statement-65ca5599f200?source=collection_archive---------8----------------------->

![](img/c179b128d9de0b30b3a3542370b676a4.png)

**if** 语句可能是您编写的第一段代码。**“If”**语句是条件语句，这意味着如果它们计算的表达式为真， **if** 语句将允许在 **if** 语句体中执行表达式。

if 语句的结构如下所示:

```
<?phpif ( boolean expression ) {
  expressions in body
}?>
```

if 语句以单词 **if** 开头，后面是一个用括号括起来的表达式。该表达式将计算为 true 或 false。如果表达式的计算结果为 true，将执行 If 语句体(通常用花括号括起来)中的表达式。

让我们看一个简单的例子。我们将通过向 if 语句中的表达式传递布尔值 true 来保证其计算结果为 true。

```
<?phpif ( true ) {
  echo "Donkey";
}?>
```

上面的代码将输出 Donkey，因为 if 语句中的布尔表达式计算结果为 true。计算结果为 true 或 false 的任何类型的表达式都可以放在条件中。

```
<?php$a = 10;if ( $a > 5 ) {
  echo "Math";
}?>
```

PHP 对上面的 if 语句进行如下评估:

1.  PHP 看到关键字 if。
2.  它寻找开始的括号。
3.  它计算括号内的表达式。存储在$a 中的值是否大于 5？换句话说，10 大于 5 吗？是的。所以，表达式是真的。
4.  由于表达式为真，PHP 查看 if 语句体内部并找到一个 echo 语句。它呼应了数学。

您也可以在布尔表达式中比较字符串。如前所述，表达式只需要计算 true 或 false。

```
<?php$word = "confused";if ( $word == "confused" ){
  echo "Bumfuzzle";
}?>
```

您还可以使用求反运算符来翻转条件语句的值。使用求反运算符，如果值为真，被求反的值将为假；如果该值为 false，则取反后的值将为 true。

什么时候需要使用求反运算符？当条件计算结果为 false 时，有时需要计算 if 语句中的表达式。在这种情况下，您可以将！运算符，然后将假翻转为真。请记住，在 if 语句中执行语句的唯一方式是条件的计算结果为 true。

```
<?php$definitelyfalse = false;if ( !$definitelyfalse ) { 
  echo "Opposite of false is true";
}?>
```

if 语句体可以包含多个表达式。

```
<?phpif ( true ) {
  $a = 3;
  $b = 5;
  $c = $a + $b; echo $c;}?>
```

如果 If 语句体中只有一个表达式，可以省略花括号。

```
<?php$oneExpression = true;if ( $oneExpression)
  echo "One Expression";?>
```

即使有多个表达式，也不一定要用花括号。PHP 提供了冒号/endif 的替代语法。

```
<?php$time = 6;if ( $time >= 6 && $time < 9 ):
  echo "Early Morning";
endif;?>
```

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/a760a23f433e1cb83f2f4574378d4b9c.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)