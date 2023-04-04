# PHP — P29: While 循环

> 原文：<https://blog.devgenius.io/php-7-x-p29-while-loops-2f178ae3d747?source=collection_archive---------7----------------------->

![](img/cfad228830054de34db707aeea21b48d.png)

如果你从未接触过循环的概念，可以这样想。假设你正在看一个 excel 表格。在 excel 表格中，有一些行在表格的下方。每一行都包含一些数据。要阅读下一行的内容，您需要向下移动。您可以一直读取每一行的内容，直到到达 excel 表格的最后一行。

使用循环，您可以从特定行读取信息，将当前行号加 1，然后移动到下一行。然后，您可以读取下一行的内容。

我们不看实际的 excel 表格，而是看一个数组。该数组可用于模拟类似 excel 表格的行和列。

我们有如下数组: *$best_cars_ever_made* 。我们已经了解了[如何显示数组](https://medium.com/dev-genius/php-7-x-p9-simple-arrays-4ef56bed0d8a)中的单个元素，但是我们如何才能一个元素接一个元素地显示，从“S13 240sx”开始，到“Subaru STi”结束

我们可以用一个循环来实现。我们将讨论许多循环，但是我们在这里只关注一个:**while 循环**。

```
while ( expression is true ) {
  do something
}
```

*while* 循环说，“当表达式为真时，在循环体内做一些事情。”如果我们将值“true”传递给条件表达式，循环将永远运行下去(只要 PHP 允许它在崩溃前运行)。像 Java 这样的编程语言确实允许你永远运行循环，实际上看到一个 *while(true)* 循环是很常见的；在 PHP 中， **always true** 循环将一直运行，直到超时；此时会返回一个错误。

最重要的一点:确保你的 while 循环最终结束。

我们在测试什么样的表情？您已经用 [if 语句](https://medium.com/dev-genius/php-7-x-p24-if-statement-65ca5599f200)完成了这种类型的测试。这只是一个条件。所以你可以这样说:

1.  while ( $x < 10 )
2.  while ( $name != “Dino” )
3.  while ( $brand == “Ferrari” || $brand == “Porsche” )

Inside of the loop body, you have your statements that execute. **你还需要有一个方法来打破条件表达式**。例如，如果您正在测试 *while( $x < 10 )* ，那么 *$x* 的值最终必须更改为大于或等于 10 的值，以便循环停止执行。

让我们看一个例子，并浏览一下。

```
<?php$x = 0;while( $x < 2 ) {
  echo $x;
  $x++;
}?>
```

在上面的例子中:

1.  PHP 将整数值 0 赋给 *$x* 。
2.  它遇到了 *while* 语句。
3.  PHP 测试条件。 *$x* 处的值是否小于 2？当前存储在 *$x* 中的值是 0。那么，0 小于 2 吗？是的。所以，进入循环体。
4.  PHP 遇到 *echo* 语句，打印出 *$x* 的值，为 0。
5.  变量 *$x* 用后递增运算符(++)从 0 递增到 1。
6.  PHP 返回到*的顶部，同时*循环并再次测试条件。 *$x* 是否小于 2？记住， *$x* 的值在*循环体中递增。变量 *$x* 现在包含值 1。那么，1 小于 2 吗？是的。移入循环体。*
7.  回显 *$x* 的值，现在是 1。
8.  增量 *$x* 。整数值 2 现在存储在 *$x* 中。
9.  PHP 返回到*的顶部，同时*循环并再次测试条件。$x 小于 2 吗？ *$x* 的值在循环体内递增，现在存储值 2。那么，2 小于 2 吗？不。跳过循环体，在循环的右花括号后继续执行。
10.  屏幕上显示的结果是: **01**

我们现在可以将相同的逻辑应用于我们的 *$best_cars_ever_made* 数组。

我们从计算数组中元素的数量开始。在 *$best_cars_ever_made* 数组中总共有 11 个元素。我们可以在循环测试中使用这些信息。我们还知道如何打印出数组中的单个元素:*echo $ best _ cars _ ever _ made[5]。为了打印第一个数组，我们需要打印索引 0 处的值。要打印最后一个值，我们需要打印索引 10 处的值。*

因此，我们将计数器变量 *$i* 初始化为 0；它也将被用作数组索引变量。为了打印出每个数组元素，我们将从 0 开始，一直到 10，因为数组中的最后一个元素在索引 10 处。我们知道我们有 11 个元素，所以我们的条件可以是 *while($i < 11)* 或 *while($i ≤ 10)* 。如果我们从 0 开始，无论如何都会在 10 结束。

在循环体内部，我们将 *$i* 的值传递给 *$best_cars_ever_made* 数组，然后*将*的值返回给返回的元素。紧接着，变量 *$i* 递增，并重复该过程。

如果我们看一下这个例子:

1.  PHP 设置 *$i = 0* 。
2.  PHP 测试看 *$i* 是否小于 11。
3.  由于 *$i* 小于 11，PHP 进入循环体。
4.  PHP 对表达式*$ best _ cars _ ever _ made[$ I]*求值。变量 *$i* 被替换为 0。
5.  PHP 获取由 *$best_cars_ever_made[0]* 返回的元素，在本例中是“S13 240sx”，并回显它。
6.  变量 *$i* 增加到 1，并重复该过程。
7.  这一直重复，直到 *$i* 被设置为 11。在第 11 次尝试时，条件打破了。
8.  由于 *< br >* 标签，输出将是一辆接一辆列出的每辆车。

如果你是动态生成数组，大多数时候数组中的元素总数是未知的。PHP 有一个助手函数可以帮助我们: *count()* 。 *count()* 函数接受一个参数，即数组，并返回数组中元素的总数。在这种情况下，*count(＄best _ cars _ ever _ made)*将返回 11。我们可以用 *count()* 函数代替 11 和我们的*，而*循环条件语句将是动态的。

我们可以通过将 *$i++* 移到 *$best_cars_ever_made* 数组的方括号内来进一步简化上面的代码。请记住，PHP 首先计算变量，然后在计算完变量后递增它。

最后要注意的一点是，如果条件从不为真，PHP 会跳过 *while* 循环。因此，对于上面的例子，如果将 *$i* 初始化为 20，PHP 将测试看 *$i* 是否小于 11。因为 20 不小于 11，所以 PHP 会跳过这个循环。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/002be1f6cd264b4dd6420d1d9f70e052.png)

迪诺·卡希奇目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。