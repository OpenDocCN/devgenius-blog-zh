# PHP — P3:变量简介

> 原文：<https://blog.devgenius.io/php-7-x-p3-variables-intro-a83ac8eb9efb?source=collection_archive---------27----------------------->

![](img/8b9764cc52960c75302d1633530cc6e5.png)

引入变量的最好方法是开始思考数学。你可能还记得，有一些叫做 x 和 y 变量的东西，用来存储数字。

如果 x = 2，y = 5，那么 x + y 是多少？用 2 代替 x，用 5 代替 y，得到 7。

变量只是存储容器。您可以在其中存储您想要的任何数据类型；下面我们来看看字符串和整数。

在 PHP 中，变量以美元符号开头，后面是下划线或字母，再后面是一系列字母、数字和/或下划线。让我们看一些有效的 PHP 变量名的例子。

```
<?php
// Valid PHP variable names
$x = 1;
$X = 2; # different from $x
$_x = 2;
$__y = 3;
$_z_ = "Hello";
$__name__ = "Dino";
$dino1 = "First Dino";
$dino_1 = "Dino 1";
$_dino_1 = "He was number 1";
$d1_2_3_hello_ = "Still valid";// Invalid PHP variable names
$1 = "Number 1";
$%abc = "Invalid"; // Really, any special character
?>
```

作为正则表达式，PHP 变量的有效语法如下:

```
^[a-zA-Z_\x80-\xff][a-zA-Z0-9_\x80-\xff]*$
```

当然，您必须熟悉正则表达式才能理解上面的代码。如果你不知道，现在就跳过它。

创建一个新文件，命名为 variables.php。复制并粘贴下面的代码，然后在浏览器中打开它。你看到了什么？你应该看看 8。

```
<?php
$x = 3;
$y = 5;echo $x + $y; // same as saying echo 3 + 5;
?>
```

先说上面的代码。整数 3 被赋给$x 变量。整数 5 被赋给$y 变量。现在我们已经将这两个值赋给了$x 和$y 变量，我们可以对这些变量做些什么了。在本例中，我们使用算术运算符+将两个变量相加，然后在屏幕上显示结果。

如果你完全是编程新手，有时看起来这个人好像跳过了一些步骤。例如，我知道我们还没有谈到我们接下来要使用的算术运算符或连接运算符，但是我们在示例中使用了它们。有些概念非常简单，你可以理解发生了什么。其他时候，您可能需要提前阅读，然后返回到上一篇文章，以完全掌握正在发生的事情。继续前进。

我们再看一个例子。我们将在变量中存储一个字符串，然后将其回显出来。首先，我们将字符串赋给$name 变量，然后将其连接到“Hi my name is”字符串。你不能只是把变量放在字符串旁边，然后期望 PHP 理解你想要连接两个字符串。您使用串联运算符(。)来做到这一点。

```
<?php
$name = "Dino Cajic";echo "Hi my name is" . $name;
?>
```

如果您在浏览器中打开代码，您会看到显示以下文本:嗨，我的名字是 Dino Cajic。您可以将$name 变量的值更改为您想要的任何值，它将自动更新输出。

让我们再做一个。看看下面的代码。我们将把汽车的年份、品牌和型号分配给$car_year、$car_brand 和$car_model 变量。由于 R34 日产天际线 GTR 在美国还不合法，我们将想要检查何时进口它将是合法的。我们将在汽车的年份上加上 25 *(汽车必须有多老才能在美国合法)*；我们还将借助 date()函数获取当前年份，并将其存储到$current_year 变量中。

下面，我们将使用一些 echo 语句在浏览器上显示变量值。

你大半辈子都在接触数学中的变量，所以你应该感觉很自在。不过变量可能会变得有点奇怪。我们将在第 12 篇和第 13 篇文章中更深入地探究变量。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/4bb81255f7b17e095a0ba2c18185da6e.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)