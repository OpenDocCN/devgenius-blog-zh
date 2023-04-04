# PHP — P10:关联数组

> 原文：<https://blog.devgenius.io/php-7-x-p10-associative-arrays-e19f11edc3ff?source=collection_archive---------7----------------------->

![](img/9bbdc5f4433d0e946f940a96ecde4093.png)

什么是关联数组？如果你熟悉其他编程语言，那么关联数组就相当于一本字典。您将使用一个键来访问数组，而不是使用一个索引值。那个键指向一个值。PHP 中的键可以是整数也可以是字符串。如果试图将浮点数存储为键，它将被转换为整数。布尔值*真*将被置为 1，而*假*将被置为 0。

我们可以用元素的索引值访问一个常规数组，但是我们必须使用分配的键值来访问一个关联数组元素。

为什么要使用关联数组而不是常规数组呢？如果我们有一个像下面这样的数组，我们如何知道一些值代表什么？我们看到 32 是数组内部的一个元素，但是 32 实际上是什么意思呢？

```
<?php$person = [
  "Dino Cajic",
  32,
  "dinocajic@gmail.com",
  42.5,
  true
];?>
```

如果我们想要指定每个值的含义，我们可以使用字符串作为我们的键，并显式地声明每个元素代表什么。

```
<?php$person = [
  "name" => "Dino Cajic",
  "age" => 32,
  "email" => "dinocajic@gmail.com",
  "fav_decimal" => 42.5,
  "is_awesome" => true,
  "occupation" => "author",
  "book_title" => "An Illustrative Introduction to Algorithms",
  "blog_link" => "https://medium.com/@dinocajic"
];?>
```

当有人现在查看数组时，很容易就能准确地找出每个元素代表什么。

我们如何使用键来访问一个元素？通过使用键而不是整数索引，就像我们对简单数组所做的那样。整数索引值，因为它没有被定义为一个键，将抛出一个错误。

```
<?phpecho $person["name"]; // displays: Dino Cajic
echo $person[0]; // Throws an error?>
```

如果我们想用一个整数作为键，我们可以，但我们可能不应该。为了向关联数组添加新元素，我们使用数组名，并在附加的括号之间指定键名。这次我们将使用一个整数值。

```
<?php$person[100] = "Hey There";
echo $person[100]; // Displays Hey There?>
```

我们可以使用 unset()函数从关联数组中删除一个元素。

```
<?phpunset( $person[100] ); // removes the Hey There elementvar_dump($person); // shows everything except Hey There.?>
```

要修改元素，请使用值所在的键，并为其分配一个新值。

```
<?php$person["age"] = 33; // replaces 32 with 33?>
```

现在我们已经介绍了关联数组，我们可以继续学习多维数组。我们将在下一篇文章中讨论这个问题。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/2ccf2b43bc4b37e8ae61f4949566baea.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)