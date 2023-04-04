# 开始在 PHP 中使用严格类型

> 原文：<https://blog.devgenius.io/start-using-strict-typing-in-php-897301e54e3d?source=collection_archive---------5----------------------->

## PHP 中什么是严格类型化？

![](img/f39161796e636c30fe9fff49835fee60.png)

由[马库斯·温克勒](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在解释 PHP 中的严格类型之前，我们要执行下面的程序。

```
<?php

function sum($a, $b)
{
  return $a + $b;
}

echo sum(1, 2.5); // output 3.5

?>
```

上面的程序是用来求两个给定数字之和的。所以输出是 3.5

## 标量类型声明

在 PHP 7.0 中引入了[标量类型声明](https://www.php.net/manual/en/migration70.new-features.php#migration70.new-features.scalar-type-declarations)。标量[类型声明](https://www.php.net/manual/en/language.types.declarations.php)有强制(默认)和严格两种模式。

现在我们要给函数参数添加类型声明。

```
<?php

function sum(int $a, int $b)
{
  return $a + $b;
}

echo sum(1, 2.5); // output 3

?>
```

现在您将得到输出 3。为什么？PHP 自动将输入参数的类型改为 int(第二个参数 2.5 浮动为 int)。这被称为强制模式。

## PHP 中什么是严格类型化？

所以根据我们的函数参数类型声明，函数应该接受一个整数值。如果传递非整数值，程序将抛出一个错误，这可以通过使类型声明严格来实现。

## 如何启用严格类型？

我们可以在每个文件的基础上启用严格模式。为了实现严格的类型化，[声明](https://www.php.net/manual/en/control-structures.declare.php)语句与`strict_types`声明一起使用。

严格类型声明只对声明的文件有效。目前没有全局应用严格类型的选项。

> 在严格模式下，只接受与类型声明完全对应的值，否则将抛出 [TypeError](https://www.php.net/manual/en/class.typeerror.php) 。

```
<?php
  declare(strict_types=1);
?>
```

向 sum 函数声明严格类型

```
<?php
declare(strict_types=1);

function sum(int $a, int $b)
{
  return $a + $b;
}

echo sum(1, 2.5);

?>
```

在声明严格类型后，您将得到下面的致命错误。

```
Fatal error: Uncaught TypeError: sum(): Argument #2 ($b) must be of type int, float given, called in C:\xampp\htdocs\test.php on line 8 and defined in C:\xampp\htdocs\test.php:4 Stack trace: #0 C:\xampp\htdocs\test.php(8): sum(1, 2.5) #1 {main} thrown in C:\xampp\htdocs\test.php on line 4
```

现在我们了解了强制模式和严格模式下类型声明的基础。

## 返回类型声明

我们也可以声明函数返回值的类型。

```
<?php
// Coercive mode
function sum(int $a, int $b): int
{
  $c = 0.5;
  return $a + $b + $c;
}

echo sum(1, 2); // Output 3

?>
```

上述程序的输出在加上$c = 0.5 后是 3。因为我们将返回类型声明为 integer。如果声明的返回类型是`float`，你将得到一个 3.5 的输出。

## 具有返回类型的严格类型化

添加严格类型后，您会得到致命错误，因为返回值不是整数。

```
<?php
declare(strict_types=1);

function sum(int $a, int $b): int
{
  $c = 0.5;
  return $a + $b + $c;
}

echo sum(1, 2); 

?>
```

```
Fatal error: Uncaught TypeError: sum(): Return value must be of type int, float returned in C:\xampp\htdocs\test.php:7 Stack trace: #0 C:\xampp\htdocs\test.php(10): sum(1, 2) #1 {main} thrown in C:\xampp\htdocs\test.php on line 7
```

## 为什么使用严格的类型？

如果在代码中使用标量类型声明，严格类型是最好的。因为有助于防止 bug。否则，不需要严格的类型声明。

## 结论

我看到很多 PHP 包开始使用严格类型。如前所述，在使用类型声明时，严格类型是避免错误的最佳方法。那么你为什么还在等待**开始使用严格类型**！

分享你对在 PHP 中使用严格类型的评论。

## 参考

*   [类型声明](https://www.php.net/manual/en/language.types.declarations.php#language.types.declarations.strict)
*   [标量类型声明](https://www.php.net/manual/en/migration70.new-features.php#migration70.new-features.scalar-type-declarations)

感谢您的阅读。

敬请关注更多内容！

*跟我来*[***balajidharma.medium.com***](https://balajidharma.medium.com/)。