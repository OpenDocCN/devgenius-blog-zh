# PHP — P2:注释

> 原文：<https://blog.devgenius.io/php-7-x-p2-comments-8e69c0b567e4?source=collection_archive---------12----------------------->

![](img/a619338bbac7b8178e09013d7566c842.png)

谈到评论，惠普有几个不同的选项。通常，这是基于你使用的评论风格的偏好，但是其他时候使用其中一种更有意义。

注释允许您编写简短或详细的描述，解释您编写的代码实际上是做什么的。你，或者其他人，将会从现在开始回到你的代码，并试图破译它的真正含义。您应该总是努力编写不言自明的代码，但是偶尔，您可能希望指定一些额外的细节。

第一种类型的注释以//开头。它们是单行注释，意味着无论你在那一行写了什么，PHP 都会忽略。

```
<?php
// This is a comment inside PHP
This will throw an error
?>
```

另一个单行注释以#开头。

```
<?php
# This is a single line comment
# This is another single line comment
# $x should be named $title
$x = "Mr";
?>
```

最后一种注释是/** */。以/**开头，以*/结尾。这可以用作单行或多行注释。任何位于/**和*/之间的字符都将被注释掉，不管这些字符跨越多少行。

```
<?php
/** Comment */
/**
  Multi-line comment
  This is also part of the comment
  $x = "Hey"; <- Still a comment
*/
?>
```

注释不必在单独的行上，因为任何在之后的**符号都被标记为注释。**

```
<?php
$x = "Mr"; // $x should be named $title
$y = "Dino"; # $y should be named $first_name
$z = "Cajic"; /** $z should be named $last_name */
?>
```

如果你看最后一条注释，它需要有结束符号 ***/** ，否则它会继续注释掉后面的任何内容。有时候当你测试的时候，使用多行注释去掉代码的某些部分会更有意义。

下面的代码对您来说是否有意义并不重要，只要知道 for 循环被注释掉了，不会被 PHP 解释。

```
<?php
$x = "Frank";***/**
for ( $i = 0; $i < 10; $i++ ) {
  echo $x . "_" . $i;
}
*/***?>
```

您还可以使用多行注释来注释掉代码的特定部分。下面的代码将注释掉“_”。$i。

```
<?php
$x = "Frank";for ( $i = 0; $i < 10; $i++ ) {
  echo $x . ***/** "_" . $i*/***;
}?>
```

按照惯例，当你做多行注释时，你将为每一个新行添加一个星号。

```
/**
 * This is a multi-line comment. 
 * 
 * Author: Dino Cajic
 * Copyright Year: 2020 
 * 
 * No. This code is not copyrighted.
 */
```

创建一个新文件，命名为 comments.php。您可以通过进入[http://localhost/comments . PHP 在浏览器中访问它。](http://localhost/comments.php.)复制并粘贴下面的代码，然后在浏览器中打开页面。查看屏幕上显示的内容，以及查看信号源时显示的内容。你会发现评论无处可寻。它们只是让你在代码中看到的。

在 [phpDocumentor](https://www.phpdoc.org/) 的帮助下，特定的注释约定也可以用来为您的 PHP 代码生成文档。我建议您也熟悉 phpDoc 在面向对象的 PHP 中，你可能会在你的整个类中看到它的使用。JetBrains PHP Storm 预装了 phpDoc。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/923ea8fc59fe02713a3bc63d38d6fa66.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)