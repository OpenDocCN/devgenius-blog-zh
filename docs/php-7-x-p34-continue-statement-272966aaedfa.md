# PHP — P34: Continue 语句

> 原文：<https://blog.devgenius.io/php-7-x-p34-continue-statement-272966aaedfa?source=collection_archive---------4----------------------->

![](img/c4cf648d2eb658d84fb748e1066d6f9e.png)

我们在前一篇文章中看到了 *break* 语句。回顾一下， *break* 语句一旦遇到就终止循环。有时我们想跳过一次迭代，继续循环；这可以通过*继续*语句来实现。

让我们看看如何在每个循环中使用 *continue* 语句: [*while*](https://medium.com/@dinocajic/php-7-x-p29-while-loops-2f178ae3d747) ， [*do-while*](https://medium.com/@dinocajic/php-7-x-p30-do-while-loop-e70777a17ee3) ， [*for*](https://medium.com/@dinocajic/php-7-x-p31-for-loop-68724b49b861) ，以及 [*foreach*](https://medium.com/@dinocajic/php-7-x-p32-foreach-loop-f38b88249e76) 。每个循环将遍历以下数组:

在第一个例子中，我们将看看 *while* 循环。PHP 将遍历 *$next_car* 数组；一旦匹配到“R34 Skyline”，PHP 就会输出“没错。我得到一个天际线。”

按照我大多数文章的习惯，我喜欢完整地浏览至少一个例子。我认为它为读者提供了一种清晰的查看代码的方式。在本例中:

1.  PHP *呼应*“While 循环”
2.  它将 *$i* 初始化为 *0。*
3.  PHP 遇到了 *while* 关键字。
4.  它看条件 *$i <计数($next_car)。*函数 *count($next_car)* 返回 5，因为数组 *$next_car 中有 5 个元素。*变量 *$i* 包含第一次迭代期间的值 *0* 。由于 *0* 小于 *5* ，表达式被评估为真。PHP 进入循环体。
5.  遇到语句时的*。PHP 检查以确保当前元素不等于“R34 Skyline”在第一次迭代中， *$next_car[$i++]* 返回“保时捷 911”一旦该语句被处理， *$i* 就从 0 迭代到 1 ( [记住后递增运算符如何工作](https://medium.com/dev-genius/php-7-x-p18-increment-and-decrement-operators-98422028d1a5))。*
6.  PHP 检查“保时捷 911”是否等于“R34 Skyline”因为没有，所以该语句的计算结果为 true，因为！=运算符。它在说，“保时捷 911 不等于 R34 Skyline 是真的吗？”PHP 回答“是的，这是真的。”
7.  由于 *if* 语句返回 true，PHP 进入 *if* 语句体，*回显* out *$i* (这只是为了向你展示 *$i* 是递增的)。
8.  PHP 遇到的下一个语句是 *continue* 语句。 *continue* 语句告诉 PHP 跳过 *while* 循环体中的剩余代码，并返回到顶部测试 *while* 循环条件。因此，回到第 4 步并重复。
9.  PHP 将在第二次和第三次迭代中遇到 *continue* 语句。
10.  在第 4 次迭代期间， *if* 语句条件评估为 *false* ，因此 *if* 语句被跳过。PHP 到达 *echo* 语句，输出“没错。我得到一个天际线。”这将是添加一个 *break* 语句来退出循环的绝佳位置，但是我们将继续下去，因为数组中可能有多个元素包含值“R34 Skyline”(并且因为我们在这里不涉及 break 语句)。
11.  因为数组中有更多的元素，并且没有做任何事情来终止循环，所以它会一直继续下去，直到到达最后一个元素。每次，如果语句有条件且*继续*语句被执行，表达式在*内返回*真*。*
12.  当 *$i* 从 5 递增到 6 时，PHP 在 *while* 循环中计算表达式，并返回 *false。* PHP 退出循环。

接下来，让我们看看 *do-while* 循环。PHP 在测试 *do-while* 条件之前，评估 *do-while* 主体内部的语句。最终，产生相同的结果。每次没有返回“R34 Skyline”时，PHP 都会遇到 *continue* 语句。一旦返回，PHP 将输出“没错。我得到一个天际线。”

让我们把它与循环示例的*稍微混合一下。我们将把“斯巴鲁 STi”添加到数组中，它恰好是我已经拥有的一辆车。我们希望循环遍历 *$next_car* 数组，并输出消息:“空白汽车肯定是一种可能性…有足够多的订户。”空白-CAR 引用每个数组元素值。如果遇到斯巴鲁 STi，既然已经拥有了(或者至少我已经拥有了)，就不要输出任何东西。因此，如果 PHP 遇到那个值，它应该使用 *continue* 语句跳过它。*

该代码实现了这一点。它遍历每个数组元素。如果值与“Subaru STi”匹配，就会遇到 *continue* 语句，PHP 会跳过循环中的剩余代码。它回到顶部，重复这个过程。否则，不会遇到*继续*语句，执行 *if* 语句下面的代码。

让我们看看最后一个循环:foreach 循环中的*。 *foreach* 循环在逻辑上与*循环的*相同(只有语法不同)。*

仅此而已。这就是在每个循环中使用 *continue* 语句的方法。这个过程对于每一个循环都是完全一样的。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/bbc7f129e56219f83e0317d54e2734ab.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体刊物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。