# PHP — P109:递归

> 原文：<https://blog.devgenius.io/php-p109-recursion-f19657ac95e9?source=collection_archive---------6----------------------->

![](img/508e44dffc27a68e55364688b6af615a.png)

在我转到 Laravel 之前，递归是 PHP 的最后一个主题。递归不是 PHP 的主题，但是我经常被问到这个问题，所以我想我们应该写一篇关于它的文章。这是一次有趣的经历，感谢您阅读这个 PHP 系列。

什么是递归？你可以深入了解它，但为了简单起见，它是一个从函数体内部调用自身的函数。实际上，这是一种解决问题的方法，通过使用函数作为解决问题的手段，从较小的问题中产生解决方案。

让我们构建一个逐步调用自身的函数。

在这个例子中，我们将创建一个接受整数的函数。我们将从`1`到`n`取每个整数，并将它们相加。一旦完成，我们将回复您。在我们使用递归函数之前，先用一个`for`循环。所以，如果我们说`n = 5`，我们将从`1 + 2 + 3 + 4 + 5 = 15`开始得到`15`。

```
<?php

// Result should be: 1 + 2 + 3 + 4 + 5 = 15
$sum = 0;
$n = 5;

for ($i = 1; $i <= $n; $i++) {
    $sum += $i;
}

var_dump($sum);
```

我们也可以用我们的`for`循环以相反的顺序进行。

```
<?php

// Result should be: 5 + 4 + 3 + 2 + 1 = 15
$sum = 0;
$n = 5;

for ($i = $n; $i > 0; $i--) {
    $sum += $i;
}

var_dump($sum);
```

现在我们可以把它可视化了，我们可以开始构建递归函数了。让我们一步一步来。首先，让我们创建函数。

```
<?php

function recursiveSum( $n ) {
    // implement recursion
}
```

我们知道我们的函数需要接收一个参数，比如`$n`，它需要将从`1`到`$n`的所有整数相加并返回总和。

我知道你在想什么。我们能不能不把 for 循环塞进这里？:)当然可以，但那不是递归函数。

如果你以前从未见过递归函数，准备好大吃一惊吧。

```
function recursiveSum( $n ) : int
{
    return $n + recursiveSum($n - 1);
}
```

这是怎么回事？让我们插入一个数字，看看会发生什么。让我们从像`$n = 3`这样的小东西开始。

```
recursiveSum( 3 );
// return 3 + recursiveSum(3 - 1) OR return 3 + recursiveSum(2)
// We're now at $n = 2
recursiveSum( 2 );
// return 2 + recursiveSum(2 - 1) OR return 3 + recursiveSum(1)
// This is where we want to stop, but our function keeps going.
```

我们还没有准备好遍历这个例子，因为递归永远不会停止。它将无限期地继续下去，或者直到 PHP 超时。我们需要阻止这一切。这意味着，如果我们把 1 作为参数传递，我们不需要做任何事情；我们只需要将 1 返回给用户。任何小于 1 的值都应该被忽略。让我们实现这两个检查。

```
function recursiveSum( $n ) : int
{
    if ($n < 1) {
        return 0;
    }

    if ($n == 1) {
        return 1;
    }

    return $n + recursiveSum($n - 1);
}
```

如果有人输入小于`1`的内容，我们可能会抛出一个错误，但是为了简单起见，我们将返回`0`。让我们来看几个场景。

```
<?php

recursiveSum(0); // returns 0
recursiveSum(-42); // returns 0
```

上面的每一个都将返回`0`，因为我们首先检查它，并且函数停止执行。让我们看看如果我们完全通过`1`会发生什么。

```
<?php

recursiveSum(1); // returns 1
```

在这种情况下，因为跳过了第一次检查，所以返回`1`。现在，当我们经过`2`时会发生什么？

```
recursiveSum(2);
// Skips both of the checks, since it passes both of them, and goes to the final step
// return 2 + recursiveSum(2 - 1) or return 2 + recursiveSum(1)
recursiveSum(1);
// We know what the recursiveSum of 1 is. We just did it, it's 1!
// Since 1 is returned we can now substitute recursiveSum(1) with 1
// i.e. return 2 + recursiveSum(1) EQUALS return 2 + 1;
// We get 3!
```

如果我们继续用`3`作为我们的论点会怎么样。

```
recursiveSum(3)
  = 3 + recursiveSum(2)
    = 2 + recursiveSum(1)
    = 2 + 1
  = 3 + 3
```

```
recursiveSum(3);
// Skips both of the checks, since it passes both of them, and goes to the final step
// return 3 + recursiveSum(3 - 1) OR return 3 + recursiveSum(2)
recursiveSum(2);
// Skips both of the checks, since it passes both of them, and goes to the final step
// return 2 + recursiveSum(2 - 1) or return 2 + recursiveSum(1)
recursiveSum(1);
// We know what the recursiveSum of 1 is. We just did it, it's 1!
// Since 1 is returned we can now substitute recursiveSum(1) with 1
// i.e. return 2 + recursiveSum(1) EQUALS return 2 + 1;
// We now know what recursiveSum(2) equals. It's 3! We substitute recursiveSum(2) with 3
// i.e. return 3 + recursiveSum(2) EQUALS return 3 + 3
// We get 6!
```

我希望你能看到这是如何深入的。另一种思考方式是这样的，如果你还是不明白的话。

```
<?php

function one() {
    return 1;
}

function two() {
    return 2;
}

function three() {
    return 3;
}

$a = one() + two() + three();
var_dump($a);
```

在这个例子中，我们有三个不同的功能:`one`、`two`和`three`。在计算时，它们都返回一个整数值。所以`$a = one() + two() + three();`产生`6`。让我们稍微改写一下。

```
<?php

function one() {
    return 1;
}

function two() {
    return 2;
}

function three() {
    return 3;
}

function result () {
    return one() + two() + three();
}

$a = result();
var_dump($a);
```

`result`函数调用其他函数。一旦它们被评估，它们被加在一起并返回结果。所以，如果函数调用自己，有什么区别。差别不大。这似乎是最难理解的概念，所以让我们再看一个例子。我们将创建一个最简单的函数，名为`testFunction`，它只返回传递给它的参数。

```
<?php

function testFunction($n) {
    return $n;
}
```

然后我们可以给它传递不同的参数，让它返回不同的结果。因此，如果我们想复制我们的递归功能，我们可以这样做。

```
function testFunction($n) {
    return $n;
}

$n = 3;
$result = testFunction($n) + testFunction($n - 1) + testFunction($n - 2);
var_dump($result);
```

我们得到了什么？

```
testFunction($n)
   = testFunction(3)
   = 3

testFunction($n - 1)
   = testFunction(3 - 1)
   = testfunction(2)
   = 2

testFunction($n - 2)
   = testFunction(3 - 2)
   = testFunction(1)
   = 1
```

结果出来到`3 + 2 + 1 = 6`。我知道在这一点上这有点过了，但是我希望你能看到当你从内部调用一个函数时会发生什么。它到达某个基本情况，然后返回。再加上其他东西的结果被返回，如此类推，直到你到达顶端。

再看一遍这个例子，自己走一遍。把它写出来，你会发现它是有意义的，如果它还没有的话。

```
function recursiveSum( $n ) : int
{
    if ($n < 1) {
        return 0;
    }

    if ($n == 1) {
        return 1;
    }

    return $n + recursiveSum($n - 1);
}
```

# 回文测验

我想我们已经准备好看一些例子了。对于第一个例子，让我们创建一个名为`isPalindrome`的函数来测试一个字符串是否是回文。

```
<?php

function isPalindrome($word) 
{
    if ( empty($word) ) {
        return true;
    }

    if ( $word[0] == $word[ strlen($word) - 1 ] ) {
        return isPalindrome( substr($word, 1, -1) );
    }

    return false;
}

var_dump( isPalindrome("") );
var_dump( isPalindrome("dino") );
var_dump( isPalindrome("dinoonid") );
var_dump( isPalindrome("dinonid") );
var_dump( isPalindrome("hello") );
```

让我们浏览一下示例。

```
isPalindrome("");
// enters the isPalindrome function.
// Since the argument $word is set to an empty string, we pass the first
// test: empty($word). Since it passes, it returns true.
```

```
isPalindrome("dino");
// enters the isPalindrome function
// It's not an empty string so it skips the first test: empty($word)
// Next, it checks whether the first character of the $word is equal to
// the last character in the $word. Since d does not equal o, it skips it.
// It sees false and returns false.
```

```
isPalindrome("dinoonid")
   = isPalindrome("inooni")
      = isPalindrome("noon")
         = isPalindrome("oo")
            = isPalindrome("")
            = true
         = true
      = true
   = true
= true 
```

*   `isPalindrome(“dinoonid”)`进入`isPalindrome`功能。
*   它不是一个空字符串，所以它跳过了第一个测试:`empty($word)`。
*   接下来，检查`$word`的第一个字符是否等于`$word`的最后一个字符。
*   由于`d`等于`d`，所以进入体内。
*   我们再次调用`isPalindrome`函数，并传递给它一个子字符串。
*   我们去掉第一个字符和最后一个字符。
*   我们叫，`isPalindrome(“inooni”)`。
*   它再次经历相同的过程，并调用`isPalindrome(“noon”)`。
*   它穿过`isPalindrome`的身体，呼叫`isPalindrome(“oo”)`。
*   它穿过`isPalindrome`的身体，呼叫`isPalindrome(“”)`。
*   这次它返回`true`。所以，我们知道`isPalindrome(“”)`是真的。
*   它返回 up，所以`isPalindrome(“oo”)`也返回`true`。
*   然后它再次上升，`isPalindrome(“noon”)`返回`true`。
*   它上升并为`isPalindrome(“inooni”)`返回`true`。
*   最后，它为`isPalindrome(“dinoonid”)`返回`true`。

我将让您自己浏览其他示例。

# 斐波那契递归函数

不如来点简单的:斐波那契数列。斐波那契公式是 F*n*= F*n-2*+F*n-1*。我们通常知道前两个斐波那契数，F *0 =* 1，F *1* = 1。有时，F *1* = 1，F *2* = 1。斐波那契是一个递归函数。我们可以说:

*   `F(4) = F(4–2) + F(4–1)`
*   `F(4) = F(2) + F(3)`
*   我们知道`F(2) = 1`，所以`F(4) = 1 + F(3)`
*   `F(4) = 1 + ( F(3–2) + F(3–1) )`
*   `F(4) = 1 + ( F(1) + F(2) )`
*   我们知道`F(1) = 1`和`F(2)`也等于`1`
*   `F(4) = 1 + ( 1 + 1 )`
*   `F(4) = 3`

```
<?php

function fibonacci($num)
{
    if ($num <= 2) {
        return 1;
    }

    return fibonacci($num - 1) + fibonacci($num - 2);
}

var_dump(fibonacci(0));
var_dump(fibonacci(1));
var_dump(fibonacci(2));
var_dump(fibonacci(3));
var_dump(fibonacci(4));
var_dump(fibonacci(5));
```

在 fibonacci 函数中，当我们调用`fibonacci(0)`、`fibonacci(1)`和`fibonacci(2)`时，我们通过了`$num ≤ 2`测试，该测试返回`1`。所以，让我们看下一个例子:`fibonacci(3)`。

```
fibonacci(3)
   = return fibonacci(2) + fibonacci(1)
   = return 1 + 1 = 2
= 2
```

下一个怎么样？

```
fibonacci(4)
   = return fibonacci(3) + fibonacci(2)
   = return fibonacci(3) + 1
      = (return fibonaci(2) + fibonacci(1)) + 1
   = (return 1 + 1) + 1
   = return 2 + 1
= 3 
```

*   我们从调用`fibonacci(4)`开始
*   从大于`2`的`$num = 4`开始，第一次测试被跳过。
*   我们输入返回语句:
    `return fibonacci($num — 1) + fibonacci($num-2)`
    `return fibonacci(4-1) + fibonacci(4-2)`
    `return fibonacci(3) + fibonacci(2)`
*   对于`fibonacci(3)`，我们通过`3`作为自变量。由于`$num = 3`大于我们的测试值`2`，我们进入下一步:
    `return fibonacci($num-1) + fibonacci($num-2)`
    `return fibonacci(3-1) + fibonacci(3-2)`
    `return fibonacci(2) + fibonacci(1)`
*   我们又要深入了。调用`fibonacci(2)`并返回`1`。
*   我们现在将`fibonacci(2)`替换为`1`，并评估下一个函数:
    `return 1 + fibonacci(1)`
*   我们输入`fibonacci(1)`，它返回`1`。
*   我们现在回到`return 1 + fibonacci(1)`，可以用`1` :
    `return 1 + 1;`代替`fibonacci(1)`
*   这在哪里被归还？以下表达式返回`fibonacci(3)`:`return fibonacci(3) + fibonacci(2)`。
*   所以我们得到`return 2 + fibonacci(2)`。
*   我们输入`fibonacci(2)`，得到`1`。
*   `fibonacci(2)`替换为`1` :
    `return 2 + fibonacci(2)`
    `return 2 + 1`
*   为`fibonacci(4)`返回值`3`。

使用递归有很多有趣的方式。当我在写我的算法书时，很难不对大多数算法使用递归。例如，下面是冒泡排序算法:

```
<?php

function bubbleSort(&$sort, $length) {

    if ($length <= 1) {
        return;
    }

    for ( $i = 0; $i < $length - 1; $i++ ) {
        if ( $sort[$i] > $sort[$i + 1] ) {
            $temp         = $sort[$i];
            $sort[$i]     = $sort[$i + 1];
            $sort[$i + 1] = $temp;
        }
    }

    bubbleSort($sort, $length - 1);
}

$array = [78,39,14,56,88,94,108,5,15];
$length = count($array);

bubbleSort($array, $length);

var_dump($array);
```

我会让你自己找出答案，或者你可以在这里阅读关于它的文章。是用 Java 写的，但是一样的东西:)。

**嗯，这就是所有的乡亲。拉勒维尔庄园见！创建这个 PHP 文章系列很有趣。希望你觉得它有教育意义。**

[](https://medium.com/geekculture/constructing-the-bubble-sort-algorithm-77fa5e908794) [## 构造冒泡排序算法

### 讲述如何在 Java 中从头开始构建冒泡排序算法的逻辑。

medium.com](https://medium.com/geekculture/constructing-the-bubble-sort-algorithm-77fa5e908794) [](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/21e575df7fd0a644e17c90d176185272.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。