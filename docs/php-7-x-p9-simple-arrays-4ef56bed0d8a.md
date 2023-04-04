# PHP — P9:简单数组

> 原文：<https://blog.devgenius.io/php-7-x-p9-simple-arrays-4ef56bed0d8a?source=collection_archive---------6----------------------->

![](img/65d3297a121ab334d6bc2b1e24ace696.png)

什么是数组？通俗地说，就是把多项一起存放在一个变量下的一种方式。你可以为每件事创建不同的变量，但有时这并不实际。假设您正在从数据库中提取数千条记录。你不能，或者不应该，创建成千上万个不同的变量来存储每条记录。您可以使用一个数组并将所有记录存储在一个变量下。然后你可以遍历数组(我知道我们还没有涉及循环)并显示每个数组元素。

在 PHP 中创建数组有不同的方法。第一种方法是将 **array()** 赋给一个变量。在 array()声明中，我们将放置一个数字列表。

```
<?php
$numbers = array(1, 2, 3, 5, 10, 20);
?>
```

这些数字有一定的用途，比如代表员工 ID。

```
<?php
$employee_ids = array(1, 2, 3, 5, 10, 20);
?>
```

数组不必只存储整数值；它们可以存储任何数据类型，例如字符串。

```
<?php
$drivers_i_hate = array(
  'Drivers that turn on their signals before checking',
  'Drivers that do the speed limit in the left two lanes',
  'Drivers that are clearly on their phones'
);
?>
```

您会注意到每个数组值都在单独的一行上。PHP 对每个数组元素后可以有多少个空格或换行符没有限制。因此，为了可读性，你可以让它看起来像你想要的那样。

我们如何访问数组内部的元素？带有索引值。什么是指数？它只是赋给每个元素的一个整数值，这样就可以调用它了。在 PHP 中，像大多数编程语言一样，我们从索引 0 开始，以 1 为增量向上移动。为什么 0 是第一个数组索引？为了不太深入，我将简单说明这是因为数组在内存中的存储方式。

在$drivers_i_hate 数组中，我们有 3 个元素；索引值为 0、1 和 2。要访问第一个数组元素，您需要获取变量$drivers_i_hate，在它后面加上几个括号[ ]，并将索引值放在括号之间。

```
<?php
echo $drivers_i_hate[0];
// Displays: Drivers that turn on their signals before checking
?>
```

为了显示最后一个元素，我们可以只计算其中的元素数，3，减去 1，因为我们的索引值从零开始，然后将整数值放在方括号内。

```
<?php
echo $drivers_i_hate[2];
// Displays: Drivers that are clearly on their phones
?>
```

如果我们不知道数组中有多少个元素呢？例如，如果我们从一个数据库中提取记录，并且该数据库表保持频繁更新，记录的数量将会改变。如果你想访问最后一个元素。因为索引值会发生变化，所以不能对其进行硬编码。我们必须以某种方式计算数组中元素的数量，并以这种方式计算出值。

幸运的是，PHP 有一个名为 [count()](https://www.php.net/manual/en/function.count.php) 的内置函数。还有一个函数叫做 [sizeof()](https://www.php.net/manual/en/function.sizeof.php) ，与 count()同义。你可以选择你喜欢的任何一个；我更喜欢数数。

让我们将数组传递给 count()函数，并获取数组中元素的数量。我们得到 3，因为这是$drivers_i_hate 数组中元素的数量。

```
<?php
echo count( $drivers_i_hate );
// Displays: 3
?>
```

根据目前已知的信息，让我们看看是否可以访问最后一个数组元素。我们所要做的就是得到数组中元素的数量，从数组中减去 1，因为索引是从 0 开始的，然后把这个值放在数组的方括号中。

```
<?php
// Stores the value 2
$last_index = count($drivers_i_hate) - 1;echo $drivers_i_hate[ $last_index ];
?>
```

如果我们愿意，我们可以将整个表达式放在方括号中；我们不必将索引值存储在变量中。

```
<?php
echo $drivers_i_hate[ count($drivers_i_hate) - 1 ];
?>
```

PHP 如何评价上面的代码？

1.  PHP 看到了 echo 语句，所以它知道它将向屏幕输出一些东西。
2.  它到达一个变量。这个变量是一个数组。
3.  PHP 在方括号中查找索引值，这样它就可以计算出需要将哪个数组元素输出到屏幕上。
4.  索引值没有显式位于方括号内；PHP 开始计算表达式，试图找出其中的索引值。
5.  它注意到有一个算术运算:左边减去右边。
6.  PHP 在左边寻找一个数字。它没有看到一个，但是它看到了 count 函数。
7.  它计算 count()函数并得到值 3。
8.  然后从 3 中减去 1，得到值 2。整数值 2 是索引值。
9.  它寻找为该索引值分配的内存位置，并检索字符串:*显然在手机上的驱动程序*。
10.  PHP 将该字符串显示在屏幕上。

还有另一种初始化数组的方法；我们可以简单地使用方括号符号，而不是使用 array()声明。*方括号符号是 PHP 5.4 中引入的。*我个人更喜欢用方括号符号来初始化数组。

```
<?php
// Set $people to empty array
$people = [];
?>
```

PHP 中的数组不需要只存储一种数据类型。这实际上是我最喜欢的 PHP 特性之一。我希望能够偶尔混合和匹配数据类型。在其他编程语言中，比如 Java，你不能这样做。

我们将创建一个名为$person 的数组，我们将存储字符串、整数、浮点数，甚至布尔值。这次让我们使用方括号符号来初始化数组。

```
<?php
$person = [
  'Dino Cajic',
  32,
  '111-11-1111', // Don't store SSN like this
  'dinocajic@gmail.com',
  42.5, // favorite decimal
  true // is awesome
];
?>
```

如果我们想看到数组中的所有值以及每个值的数据类型，我们可以使用 PHP 内置的 [var_dump()](https://www.php.net/manual/en/function.var-dump.php) 函数。var_dump()函数实际上只是转储关于变量的信息。

```
<?php
var_dump($person);
?>
```

![](img/4a7753981dfef132eb6a6d31c2c1c612.png)

如果我们想在数组初始化后添加一个新元素呢？有几种方法可供你选择。我们可以通过使用数组名称将数组元素添加到数组的末尾，将方括号附加到名称的末尾，将最后一个索引值+ 1 插入括号中，最后将值赋给它。

我们将在索引 6 下的$person 数组中存储这个人的职业。为什么是 6？因为索引 0 到 5 已经被占用。

```
<?php
$person[6] = "Author";
?>
```

如果您想将值放入数组中，而不用担心最后一个数组元素是什么，您可以简单地使用开/闭括号。PHP 会自动将元素赋给数组中的最后一个索引值。

```
<?php
// Book title stored in index 7
$person[] = "An Illustrative Introduction to Algorithms";
?>
```

在给数组分配元素时，也不必按照数字顺序。$person 数组当前使用的索引是 0 到 7。我们可以使用数组索引 26 作为我们的下一个元素位置，它会把它分配给那个索引。元素 8 到 25 将不存在。

```
<?php
$person[26] = "https://medium.com/@dinocajic";
?>
```

![](img/d9611e57b25d0fb77fdda443bdab525e.png)

如果我们将$person 数组传递给 count 函数，这个数组的大小将是 9。这意味着我们不能使用之前使用的技巧来获取数组的最后一个元素。我们将继续，像以前一样，从 9 中减去 1，我们的索引值将是 8。不幸的是，我们需要 26 个。我建议，除非你有一个很好的理由来创建自定义索引值，否则不要像我们刚才做的那样随意分配索引值。

我们如何修改现有的元素？首先，我们必须知道元素的索引值。然后，我们为该索引分配一个新值。

```
<?php
// To modify the age from 32 to 33
// New value for element at index 1 is 33
$person[1] = 33;
?>
```

要移除数组元素，我们将使用内置的 unset()函数。我们将把数组元素传递给它，它将把它从数组中移除。如果我们想删除刚刚添加的最后一个元素，即索引为 26 的元素，并将其作为数组中的最后一个元素重新插入，我们可以这样做，但是会得到一些意外的结果。

```
<?php
// Removes array element 26 from the array
unset( $person[26] );// What array index will the link be added to?
$person[] = "https://medium.com/@dinocajic";
?>
```

在我们将字符串[*【https://medium.com/@dinocajic】*](https://medium.com/@dinocajic)*重新插入$person 数组*，*之前，我们可以验证数组中的最后一个元素位于索引 7 处。插入后，链接将位于索引 27 处。事实证明，PHP 记得最后一个索引值的位置，即使在它被删除之后。*

*[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials)* *![](img/85bc7e1db3e4ce714c6c8fcb0ec9a88a.png)*

*迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。*

*你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。*

*[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)*