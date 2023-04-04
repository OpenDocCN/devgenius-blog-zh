# PHP — P11:多维数组

> 原文：<https://blog.devgenius.io/php-7-x-p11-multidimensional-arrays-e755da44eb68?source=collection_archive---------13----------------------->

![](img/91729e3ac51759da40b49870d43ce94f.png)

什么是多维数组？想象一下数组中的数组。换句话说，它是一个将数组存储为元素的数组。那些本身是数组的元素可以将数组作为元素。解释这一点最简单的方法是用一些例子。在上一篇关于关联数组的文章中，我们研究了$person 数组。

$person 数组包含一些键/值对，比如人名、年龄、电子邮件、职业和 book_title。book_title 代表作者写的书。我认为，如果一个人把写作作为他或她的职业，他或她最终会有不止一本书。我们如何在$person 数组中存储这些书呢？我们可以为每本新书创建一把新钥匙。

但是，每次作者写新书时，我们都必须修改$person 数组。现在想象一下，一家销售书籍的公司想要为他们列出的每个作者创建一个数组。有些人出版了 1 本书，而其他人出版了数百本。为了保证每个作者在$person 数组中都有一本书，公司必须向每个作者提供最大数量的书。假设书的最大数量是 100。写了 100 本书的作者将具有从 book_title 到 book_title_100 的键。写了 1 本书的作者也会有相同的密钥。如果写了 100 本书的作者又写了一本书，你就必须再次修改每个作者。这似乎浪费了很多时间和空间。

幸运的是，我们可以将所有的书籍存储在一个数组中，然后用一个名为 books 的键指向该数组。

在 PHP 中，你可以在数组中混合数据类型，这意味着你可以将数组数据类型与字符串、整数、布尔等数据类型一起存储。在一些编程语言中，如果你想将一个数组存储为一个元素，数组中的每个元素都必须是一个数组。而不仅仅是任意的数组；它必须是一个元素数量相同的数组。

在 PHP 中，你也可以在关联数组中存储常规数组。如果你看看我们的 books 数组元素，它只包含值。那么，如何从 books 数组中访问数组元素呢？您只需为每个新维度添加括号。

```
<?php
echo $person["books"][2]; // Prints: A Book on PHP
?>
```

因为 books 数组是一个没有定义键的数组，所以我们只使用索引。调用$person["books"]将返回整个 books 数组。为了访问数组中的一个元素，我们给它加上一对括号，并指定我们想要访问的数组元素。

现在我们已经了解了基础知识，让我们先深入一下，克服对多维数组的恐惧。我们要深入几层。如果你知道如何深入 2 维空间，你应该知道如何深入 50 维空间。我们将从创建一个简单的$car 关联数组开始。最初它会有年份，品牌和型号。

```
<?php$car = [
  'year'  => 2020,
  'make'  => 'Nissan',
  'model' => 'GTR'
];?>
```

大部分车型都有不同的饰件，不同车型和汽车品牌的饰件数量也不一样。对于这个$car，我们将添加另一个关联数组作为 trims 的元素。

每个修剪都指向另一个数组。我们将在特定的 trim 数组中存储每个 trim 的详细信息，例如价格和规格。

价格相当便宜。每个 trim (premium、50 周年和 track_edition)都有一个关联数组，关联数组中的第一个元素是价格。price 键指向一个字符串值。有多个规格，比如马力和扭矩，需要存储在另一个数组中。规格键指向规格数组。

马力(hp)和扭矩键指向整数值，但是燃料经济性指向一个数组，因为我们可以有高速公路、城市，甚至所需的汽油类型。

对于燃油经济性关联数组，我们有指向字符串或浮点值的类型、城市和公路键。好吧，我们建立了这个巨大的多维数组。我们现在如何获取价值？还记得我说过要在主数组后面加上括号。因此，让我们看看如何获取高级装饰的价格。

1.  从 **$car** 数组开始。
2.  找到高级装饰所在的位置。看起来 **trims** 指向一个包含 **premium** 键的数组。
3.  将**饰件**追加到 **$car:** $car['trims']。现在，您可以访问 trims 指向的数组。
4.  将**溢价**追加到**$ car[' trims ']:**$ car[' trims '][' premium ']。现在，您可以访问 premium 指向的阵列。
5.  **premium** 数组中的第一个元素是 **price** 。将**价格**追加到你的链条末端:$ car[' trims '][' premium '][' price ']。

当然，您可以重复这一点，因为 price 是以字符串形式存储的。

```
<?phpecho $car['trims']['premium']['price'];?>
```

如果我们想知道赛道版车型的城市 MPG 呢？再次遵循同样的方法，继续进入数组。

$ car[' trims '][' track _ edition '][' specs '][' fuel _ economy '][' city ']

Car 包含一个键 **trims** 指向一个数组。 **trims** 数组包含一个指向数组的关键字 **track_edition、**。 **track_edition** 数组包含一个键， **specs，**指向一个数组。**规格**数组包含一个指向数组的键**燃料经济性**。 **fuel_economy** 数组包含一个键**city**，它指向一个浮点数。

当你第一次看到它时，似乎很可怕，但是只要你有策略地深入阵列，它就相对简单。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/06fa12125b00ca91716aa776fcd8ee68.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)