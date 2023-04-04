# PHP — P7:浮点/双精度

> 原文：<https://blog.devgenius.io/php-7-x-p7-floats-doubles-cc29425f313e?source=collection_archive---------13----------------------->

![](img/e9032bd3853b045fe7561575e070e7d8.png)

浮点和双精度在 PHP 中是一回事。下列数字被认为是浮点数。

```
<?php
$a = 1.234;
$b = 1.2e3;
$c = 7E-10;
?>
```

PHP 中的浮点精度最高可达 14 位。这包括句点之前和之后的累积位数。我们将创建两个总共 14 位的变量和一个 15 位的变量。我们将它们相加，看看我们得到的结果。

```
<?php
// 14 digits
$a = 1.0000000000001;
$b = 0.0000000000001;// 15 digits
$c = 0.00000000000001;echo "A: " . $a . "<br>";
echo "A + B: " . $a + $b . "<br>";
echo "A + C: " . $a + $c . "<br>";?>
```

我们得到的结果是:

答:1.0000000000001；
A+B:1.000000000002；
A+C:1.000000000001；

前两个结果正确，最后一个 A + C 不正确。结果应该是:1.0000000000011，但最后一个 1 被剪掉了。因为浮点数的最大精度是 14，所以第 15 个数字会被截断。

如果我们将变量$c 中的最后一个数字修改为等于 6 而不是 1，那么新的结果 A + C 将等于 1.000000000002。

```
<?php
// 14 digits
$a = 1.0000000000001;
$b = 0.0000000000001;// 15 digits
$c = 0.00000000000006;echo "A: " . $a . "<br>";
echo "A + B: " . $a + $b . "<br>";
echo "A + C: " . $a + $c . "<br>";?>
```

答:1.0000000000001；
A+B:1.000000000002；
A+C:1.000000000002；

当涉及浮点比较时，也有不一致的地方。最好用一个例子来说明这一点。

```
<?php$x = 8 - 6.4; // Should be 1.6
$y = 1.6;if ($x == $y) {
  echo "They equal";
} else {
  echo "They don't equal";
}?>
```

如果我们运行代码，它会显示“它们不相等”他们应该，因为 8 减 6.4 等于 1.6。这不是 PHP 的问题；一般来说，浮点数在计算机中的存储方式有问题。

我知道我们还没有讨论函数，但是我觉得有必要简单讨论一下，因为这是一篇关于浮点数的文章。稍后将会有一系列关于函数的文章。

PHP 7 允许类型提示。在参数声明中，可以通过参数指定函数将接受的数据类型。Float 是 PHP 中允许的可接受的类型提示声明；double 被视为类名。如果你尝试用 double 输入提示，你会得到一个错误。下面是一个使用浮点数进行类型提示的例子。

```
<?phpfunction does_if_float( float $a ) {
  var_dump( $a );
}does_it_float(1.0001);?>
```

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/6b010532fe53bfcabe976f9672c5f520.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)