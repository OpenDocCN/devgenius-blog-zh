# PHP — P12:变量

> 原文：<https://blog.devgenius.io/php-7-x-p12-variables-5eef8f90ee4b?source=collection_archive---------26----------------------->

![](img/3e7c68f24dc0ae8680811f71458f171f.png)

我在[变量介绍文章](https://medium.com/dev-genius/php-7-x-p3-variables-intro-a83ac8eb9efb)中写了关于变量的内容，但是我将在接下来的两篇文章中进一步解释变量。变量三篇？是的。有太多的材料需要被覆盖。

重述:在 PHP 中，变量以美元符号开头，后面是下划线或字母，再后面是一系列字母、数字和/或下划线。它们区分大小写，所以$awesome、$Awesome、$aWesome 和$AWesome 都是不同的变量。

PHP 允许你给一个变量赋一个值，然后用完全不同的数据类型给另一个值重新赋值；大多数其他编程语言不允许这样做。

```
<?php
$x = "Dino";
$x = 1;
$x = true;
?>
```

到目前为止，很标准的东西。但我想更深入地挖掘，从计算机科学的角度探索变量。我的目标是让你了解幕后发生的事情。

假设我们想要复制一个变量。有两种不同的方法:通过**值**复制和通过**引用复制。**

按值复制变量是什么意思？为了回答这个问题，我们将创建一个新变量。

```
<?php
$name = "Dino Cajic";
?>
```

“Dino Cajic”是一个存储在内存中的值。我们说它存储在内存位置 0x12FF。变量$name 指向那个内存位置。当您调用$name 变量时，它会从该内存位置检索值，即“Dino Cajic”这是对实际过程的过度简化。

如果我们创建了一个新的变量$name_2，我们想通过设置$name_2 = $name 来复制值“Dino Cajic”，那么在内存中会发生什么？

```
<?php
$name = "Dino Cajic";
$name_2 = $name; // $name_2 = "Dino Cajic"
?>
```

1.  PHP 查看$name 变量。它进入它所指向的内存地址并获取值。因此，它查看存储器地址 0x12FF 并检索值“Dino Cajic”
2.  然后，它将值“Dino Cajic”分配给$name_2。怎么会？将值“Dino Cajic”存储在新的内存位置 0x34FD 中，并将变量$name_2 指向该新的内存位置。
3.  变量$name 现在指向 0x12FF，变量$name_2 指向 0x34FD。值是相同的，但是在内存中的位置不同。

如果我们修改变量$name_2 来存储“斯蒂芬·约翰森”，那么只有内存位置 0x34FD 中的值被修改。$name 的值是“Dino Cajic”，而$name_2 的值是“斯蒂芬·约翰森”这就是我们所说的“按值复制”的含义

```
<?php
echo $name; // Display: Dino Cajic
echo $name_2; // Displays: Stephen Johnson
?>
```

接下来让我们处理“引用复制”。我们将创建一个新变量$name_3，这一次，我们希望$name_3 指向与$name 相同的内存位置。$name 和$name_3 都需要指向内存位置 0x12FF。

```
<?php
$name_3 = &$name;
?>
```

**&** 符号是告诉 PHP 我们想要通过引用来复制的。从技术上讲，变量只是存储内存位置，所以我们只是将内存位置从＄name(0x 12ff)复制到 name _ 3。因为$name 和$name_3 现在都指向同一个内存位置，所以修改其中一个值就会修改这两个值。

我知道我们还没有谈到函数，但是对于那些对它们感兴趣和熟悉的人来说，您可能会喜欢下一部分。

向函数传递参数时，我们可以通过值或引用传递参数。通过用&符号预先挂起参数声明，可以采用相同的方法。当通过值传递参数时，会进行复制。当函数结束时，对该值的任何修改也结束。当通过引用传递参数时，对函数外部的变量进行修改。

为了说明这一点，我们将创建一个新变量$name。我们将存储值“Dino”并将该值传递给函数 displayName($name)。该函数将修改函数体内的变量，并将值改为“Frank”当我们在函数外回显$name 时，显示的值仍然是“Dino”

如果我们将$name 变量传递给 displayNameByRef(&$name)并修改函数内部的$name 值，那么当我们在函数外部回显$name 变量时，我们会得到与以前不同的结果。如果该函数将$name 的值从“Dino”更改为“Frank”，并且该变量在函数外部出现，则显示的值为“Frank”

PHP 中的变量可能会有点奇怪。我们将在下一篇文章中处理可变变量。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/d4f5119a395d787259e022174786c0c6.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。