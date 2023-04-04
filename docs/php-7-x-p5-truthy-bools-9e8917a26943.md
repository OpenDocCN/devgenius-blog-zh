# PHP — P5:真布尔

> 原文：<https://blog.devgenius.io/php-7-x-p5-truthy-bools-9e8917a26943?source=collection_archive---------10----------------------->

![](img/b3321ba0f9cb28978f36028edb2a30a0.png)

我们在[的上一篇文章](https://medium.com/@dinocajic/php-7-x-p4-basic-booleans-40cf0c0140e8)中看到了布尔值。布尔表示真或假。PHP 有真布尔值，它们不是真或假的值，但是当在表达式中求值时，它们可以表现得像真或假一样。

最好的例子是整数 0 和 1。如果我们将 1 传递给 If 语句，PHP 会将数字 1 解释为真，并在语句体内部执行该语句。如果将 0 传递给 If 语句，PHP 会将 0 解释为 falsy，因此它不会执行 if 语句体中的语句。

```
<?php
if ( 1 ) {
  echo "This is a truthy value";
}if ( 0 ) {
  echo "This is a falsy value";
}
?>
```

事实上，我们可以添加任何整数值，正的或负的，PHP 将把它们解释为真值。它只将 0 解释为 falsy。

```
<?php
if ( -5 ) {
  echo "This statement will execute";
}
?>
```

还有其他被认为是真实和虚假的价值观。空字符串""被认为是错误的。

```
<?php
$camera = "";if ( $my_camera ) {
  echo "This statement won't execute";
}
?>
```

非空字符串实际上被认为是真的。如果你有一个字符串里面有一个值，比如“随机的”，那么这个语句将被计算为真。

```
<?php
$camera = "EOS R";if ( $camera ) {
  // This statement executes
  echo "You have a nice camera";
}
?>
```

即使我们将字符串“false”传递给 if 语句，它仍然被认为是真的。

```
<?php
$some_string = "false";if ( $some_string ) {
  echo "Yup, this will execute";
}
?>
```

我知道我们还没有涉及数组，但是数组只是一个存储容器，可以存储多个值，比如多个字符串。

```
<?php
$cars = []; // empty arrayif ( $cars ) {
  echo "Empty arrays are considered falsy";
}
?>
```

空数组本质上被认为是错误的，所以主体中的语句不会被执行。非空数组被认为是真的，所以体中的语句将被执行。

```
<?php
$cars = ['MKIV Supra', 'R34 GTR', 'WRX STi'];if ( $cars ) {
  echo "What a car collection!";
}
?>
```

开发人员有时更喜欢像上面这样的真实表达式来测试和查看数组中是否有任何东西。如果有，执行一些代码；我更喜欢显式检查。PHP 有一个 count()函数，计算数组中元素的数量。然后我们可以使用比较操作符，并显式地检查数组中是否有一个或多个元素。如果有，那么执行代码。如果没有，跳过它。

```
<?php
$cars = ['3000GT', 'S13 240sx', '240z'];if ( count($cars) > 0 ) {
  echo "Another impressive collection.";
}
?>
```

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/3134c23c00b0d4d4b58a274f6fa4db5a.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)