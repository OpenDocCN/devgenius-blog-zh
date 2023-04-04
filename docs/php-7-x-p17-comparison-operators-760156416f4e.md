# PHP — P17:比较运算符

> 原文：<https://blog.devgenius.io/php-7-x-p17-comparison-operators-760156416f4e?source=collection_archive---------17----------------------->

![](img/891be152bf8a104ac73c83626a49912b.png)

比较运算符或关系运算符测试两个值之间的某种关系。我们在数学中见过一些比较运算符，比如大于号和小于号；在 PHP 中，我们有更多的比较操作符。

我们要看的关系运算符是:

*   大于: >
*   大于或等于:≥
*   小于:<
*   Less-than-or-equal-to: ≤
*   Equal-to: ==
*   Identical-to: ===
*   Not-equal-to: != or <>
*   不完全相同！==
*   飞船操作员:<=>

无论您选择哪个比较运算符，其计算结果都将是 true 或 false。如果我们说，3 > 2，我们实际上说的是:

> 3 大于 2 吗？

如果我们可以说**是**，那么这就评估为**真**陈述。如果我们说**不**，那么这将评估为**假**语句。

让我们从**给**一些变量赋值开始。记住，**单个**等号表示赋值；**双**等号表示相等。

```
<?php$a = 10;
$b = 5;
$c = "10";?>
```

我们将使用 var_dump()函数来帮助我们判断表达式的计算结果是真还是假；它只会转储表达式的结果。

让我们从等式运算符开始，==。我们想看看$a 是否等于$b，10 是否等于 5？没有，所以是假的。

```
<?php
var_dump( $a == $b ); // false
?>
```

让我们注意每个变量的不同数据类型。

*   $a 存储一个整数
*   $b 存储一个整数
*   $c 存储一个字符串

让我们再次尝试等式操作符，这一次是在$a 和$c 之间。为什么？PHP 查看字符串“10”，看到里面包含一个数字；这是一个数字字符串。它粗略地比较了这两个值，发现它们确实匹配。

> *相等运算符只查看值，不查看数据类型。*

```
<?php
var_dump($a == $c); // true
?>
```

如果数据类型很重要，则使用标识运算符===。它比较值和数据类型。如果不匹配，表达式将返回 false。

```
<?php
var_dump($a === $c); // false
?>
```

同样，如果您想查看两个值是否不相等，可以使用“不等于”运算符(！=).如果两个值不相等，则返回 true。记住，我们要问的是:10 不等于 5 吗？没错，10 不等于 5，所以这是一个正确的陈述。

```
<?php
var_dump($a != $b); // true
?>
```

您也可以使用<>来测试不平等。

```
<?php
var_dump($a <> $b); // true
?>
```

不等于运算符只测试表达式的值，而不测试数据类型。为了测试数据类型以及值，我们将使用 not-identity-to 运算符(！idspnonenote)。==).

```
<?php
var_dump($a != $c); // false
var_dump($a !== $c); // true
?>
```

大于、小于、大于或等于和小于或等于是直截了当的。我相信您在生活中的某个时候已经看到过这些操作符在起作用。

```
<?php
var_dump($a > $b); // true
var_dump($a < $b); // falsevar_dump($a >= $c); // true
var_dump($a <= $c); // true
?>
```

没有大于等于运算符或小于等于运算符。

最后一个操作员叫飞船操作员(<=>)。当值相等时，它返回 0，当右侧大于左侧时返回 1，当左侧大于右侧时返回-1。

*   等于时为 0
*   1 当右侧较大时
*   -1 当左侧较大时

```
<?php
var_dump($a <=> $b); // -1
?>
```

虽然我们看的是整数值，但是我们可以比较任何数据类型。例如，我们可以将 **$a** 与布尔值 **true** 进行比较。我们得到一个奇怪的结果:真的。为什么？除了零以外的任何整数，当与 true 比较时，都等于 true。如果您不小心将一个数字传递给一个条件语句，比如 if 语句，这可能会导致一些问题。我们稍后再看。

```
<?php
var_dump($a == true); // true
var_dump($a === true); // falsevar_dump(4.5 > 2); // true
var_dump("Dino" == "dino"); // false
var_dump("Dino" == "Dino"); // true
?>
```

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/7c4bb41c7d7c6fa5ab7cebd38c351240.png)

迪诺·卡希奇目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。