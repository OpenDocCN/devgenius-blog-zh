# PHP — P26: elseif 语句

> 原文：<https://blog.devgenius.io/php-7-x-p26-elseif-statement-adfa41f399?source=collection_archive---------8----------------------->

![](img/da376ffeb84c030b1d8ed1d88437a963.png)

当你的决策有两种以上的结果时，你需要使用 *elseif* 语句。 *elseif* 关键字位于 *if* 和 *else* 关键字之间。您的主要条件由 *if* 语句处理。如果条件在 *if* 语句中得到满足，那么测试任何其他条件都没有意义。执行 if 语句体中的代码，跳过其余的 *elseif/else* 语句。

当条件不满足时，PHP 接下来将检查 *elseif* 语句。如果条件匹配，特定的 *elseif* 语句体中的代码将会执行，PHP 将会跳过 *elseif/else* 测试的剩余部分。

如果不满足任何条件，PHP 将执行 *else* 语句体中的代码。

*if/elseif/else* 语句的结构如下:

```
<?phpif ( conditional expression ) {
  expression
} elseif ( conditional expression ) {
  expression
} else {
  expression
}?>
```

是时候举个简单的例子了。如果我们做的一切都是正确的，那么只有一个表达式将在 *if/elseif/else* 结构中执行。

```
<?php$car = "Corvette";if ( $car == "Subaru" ) {
  echo "Subie Wave";
} elseif ( $car == "Jeep" ) {
  echo "Jeep Wave";
} elseif ( $car == "Corvette" ) {
  echo "Corvette Wave";
} else {
  echo "No Wave";
}?>
```

让我们看看 PHP 在评估上面的代码时做了什么。

1.  PHP 将值 *Corvette* 赋给变量 *$car* 。
2.  PHP 遇到了 *if* 语句。它测试存储在 *$car* 变量中的值是否等于 *Subaru。*既然不是，它就继续执行 *elseif* 语句。
3.  它测试查看存储在 *$car* 变量中的值是否等于 *Jeep* 。既然不是，它就继续下一条语句。
4.  PHP 测试查看存储在 *$car* 变量中的值是否等于 *Corvette* 。确实是。
5.  PHP 执行那个 *elseif* 语句中的代码。PHP 输出*克尔维特波。*
6.  它跳过了 *else* 语句，因为它已经找到了一个匹配。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/01fd046fb343a3c52a4969acb1ab20cc.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)