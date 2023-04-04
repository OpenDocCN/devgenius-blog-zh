# PHP — P18:递增和递减运算符

> 原文：<https://blog.devgenius.io/php-7-x-p18-increment-and-decrement-operators-98422028d1a5?source=collection_archive---------16----------------------->

![](img/9a7e6a68579a721e38b50b351825708e.png)

PHP 包含增量和减量操作符。这些运算符可以在当前值的基础上加 1 或减 1。操作符可以是前缀或后缀，每次产生的结果会略有不同。

*   增量运算符:++
*   减量运算符:-

通常，要给一个值加 1，可以使用加法运算符。

```
<?php$a = 5;
$a = $a + 1;
var_dump($a);?>
```

在上面的例子中，您计算右边的表达式$a + 1，它变成 6，然后您将该值赋回左边的$a。

使用后缀-增量操作符，我们可以在变量的末尾添加++并且它会做同样的操作。

```
<?php$b = 5; 
var_dump($b++); // still displays 5
var_dump($b); // displays 6?>
```

但是如果我们对变量求值，我们会得到一个意想不到的结果:5。如果我们在值上加 1，为什么它仍然显示 5 而不是 6？使用后缀-增量运算符，首先显示值，然后执行运算。如果我们调用后一个值，它将显示前一个值加 1。

使用前缀增量运算符，首先计算表达式，然后显示。

```
<?php$c = 5;
var_dump(++$c); // displays 6
var_dump($c); // still displays 6?>
```

只有在表达式内部执行运算或者将变量作为参数传递时，才需要记住这一点。如果您递增该值，然后调用它，无论您是在运算符前还是后添加前缀，结果都是一样的。

```
<?php$a = 5;
$a++;
echo $a; // prints 6$b = 5;
++$b;
echo $b; // prints 6$c = 5;
echo $c++; // prints 5
echo $c; // prints 6$d = 5;
echo ++$d; // prints 6?>
```

同样的原理也适用于 decrement 运算符，但不是在值上加 1，而是减 1。

```
<?php$a = 5;
$a--;
echo $a; // prints 4$b = 5;
--$b;
echo $b; // prints 4$c = 5;
echo $c--; prints 5
echo $c; prints 4$d = 5;
echo --$d; // prints 4?>
```

递增和递减操作符在循环中被大量使用，我们还没有用到，但是很快就会用到。接下来我们将看看逻辑运算符。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/4556d161c4f3023e0790de89e8212dff.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。