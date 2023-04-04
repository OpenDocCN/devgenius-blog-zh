# PHP — P107:范围怪癖

> 原文：<https://blog.devgenius.io/php-p107-scope-quirks-93b188004952?source=collection_archive---------19----------------------->

![](img/8ba172a21e8d1f7aa138c18d5da0e550.png)

作用域是程序的某一部分所拥有的变量和函数的可见性。例如，假设我们在函数内部声明了一个变量。其他函数将无法访问该变量。

```
<?php

function hello() {
    $name = "Dino";
    echo "Hello " . $name; // has access
}

function bye() {
    echo "Bye " . $name; // Doesn't have access
}
```

该变量(`$name`)具有创建它的函数的局部作用域。变量可以具有全局范围，并且可以在任何地方访问(不总是立即访问)。这些变量通常在函数之外定义。

```
<?php

$x = "Hey";

function hey() {
  echo "Hey";
}

echo $x; // works well
```

然而，在 PHP 中，在函数内部调用具有全局范围的变量是不允许的，因为函数会寻找局部变量。

```
<?php

$x = "Hey";

function hey() {
  echo $x; // Doesn't work since it's looking for a local var
}
```

# 使用关键字

那么我们如何访问这个变量呢？还记得我们的`use`关键词吗？如果没有，在这里看一下。

[](/php-7-x-p40-use-keyword-37d8e7df9138) [## PHP — P40:使用关键字

### 当函数被定义时，Use 获取全局变量的值，而 global 将获取变量的值…

blog.devgenius.io](/php-7-x-p40-use-keyword-37d8e7df9138) 

我们在那篇文章中使用的例子是:

```
<?php

$name = "Dino";

$hello = function() use($name) {
    echo $name;
};

echo $name; // Dino

$name = "Harrison";

echo $name; // Harrison
$hello();   // Dino
```

这是一个关于 scope 如何工作的很好的例子。但是，我们在第 11 行将 *$name* 的值改为 *Harrison* ，那么为什么会显示 *Dino* ？

> **使用***获取全局变量的值当* **函数被定义** *和* **全局***将获取变量的值当* **函数被调用**

当 PHP 读取程序时，它在第 5 行定义了函数，并复制了全局变量的值，以便可以使用它。后来，在第 14 行，闭包被调用。已经执行了定义部分，并且值已经固化在闭包内部。这就是为什么你得到的值是*迪诺*而不是*哈里森*。

让我们看看那篇文章的另一个例子。这一次，我们同时使用了参数和传递全局变量

```
<?php

$name1 = "Dino";
$name2 = "Harrison";

$closure = function($greeting) use($name1) {
    global $name2;

    echo $greeting . " " . $name1 . " and " . $name2 . "<br>";
};

$closure("Hello there");

$name1 = "Amy";
$name2 = "Steve";

$closure("Hi there");
```

那么我们得到了什么？

```
Hello there Dino and Harrison
Hi there Dino and Steve
```

即使两个全局变量都被更改，就匿名函数而言，只有`$name2`被更新，因为当**函数被定义时*使用*获得全局变量的值，当**函数被调用时**和**全局*获得变量的值。*

# 全局关键字

有了`global`关键词，我们就可以绕过`use`关键词的不一致性。您可能想要这种格式，但是如果您不想要，`global`是一个不错的选择。

```
<?php

$a = 10;
$b = 20;

function sum()
{
    global $a, $b;

    $b = $a + $b;
}

sum();
var_dump($a);
var_dump($b);

$a = 0;
$b = 0;
sum();
var_dump($a);
var_dump($b);
```

这里发生了什么？

1.  PHP 将`10`分配给`$a`，将`20`分配给`$b`。
2.  一个函数被声明为`sum`，我们将`$a`和`$b`设置为函数中的`global`变量。
3.  调用`sum`函数。
4.  将变量`$a`和`$b`相加，并将值赋回`$b`。
5.  `$b`现在等于`30`。
6.  我们检查`$a`和`$b`的值。它们是`10`和`30`。
7.  `$a`和`$b`现在被设置为`0`。
8.  再次调用`sum`函数。
9.  一旦我们检查了`$a`和`$b`的值，两者的值都是`0`，这是我们通常认为会发生的情况。

我们不必使用`global`关键字来使变量全局化。我们可以使用`$GLOBALS`内置数组。每当定义一个全局变量时，变量的名称被设置为`$GLOBALS`数组中的键。

```
<?php

function sum()
{
    $GLOBALS['b'] = $GLOBALS['a'] + $GLOBALS['b'];
}
```

![](img/82e4cce617799ec1ab6f1d7a553c1a30.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 主管，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。