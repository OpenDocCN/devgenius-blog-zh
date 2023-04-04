# PHP — P23:运算符优先级

> 原文：<https://blog.devgenius.io/php-7-x-p23-operator-precedence-c7584f9dd79f?source=collection_archive---------22----------------------->

![](img/b201587a70a336780c4e1a14501a8c80.png)

运算符优先级只是查看将首先执行哪个操作。你已经看到了数学中的运算符优先级。乘法和除法运算符在加法和减法运算符之前执行。对于 PHP 中所有不同的操作符，解释器需要知道以什么顺序执行哪个操作。

让我们从一个基本的数学例子开始:

**2 + 3 * 5**

如果你不知道运算符在数学中的优先顺序，你可能会从左向右开始计算。你得到的答案将是不正确的:25。然而，我们都学过数学，我们知道乘法在加法之前，所以我们得到 17。

你不必知道每个操作符的优先级:如果你遇到奇怪的事情，你可以随时查找。但是，您应该熟悉以下这些:

*   新的
*   **(指数)
*   ++ -(增量/减量)
*   ！(逻辑非)
*   * / %(算术)
*   + - .(算术/连接)
*   == != === !== <> <=>(比较)
*   &&(逻辑)
*   ||(逻辑)
*   ？:(三进制)
*   = += -= *= /= %=(赋值)
*   与(逻辑)
*   异或(逻辑)
*   或(逻辑)

为什么运算符优先级如此重要？你已经知道数学中哪些运算会分开，那有什么大不了的？它可能给你带来最多问题的时候是在逻辑操作期间。

让我们看看下面的例子:

```
<?phpvar_dump( true || !true && false );?>
```

如果我们从左到右工作，我们可以评估表达式如下

1.  真||！真实的
2.  真||假=真
3.  真&&假=假

你得到的结果是假的。为了使 OR (||)运算符为真，左侧或右侧必须为真(或两者都为真)。既然左边是真的，那么整体表达也是真的。要使 AND (&&)运算符返回 true，两边都必须为 true。既然右边是假的，那么整体表达也是假的。

当然，我们收到了错误的结果。让我们看看当我们考虑操作符优先级时的结果。上面表达式中的运算符是！、&&、和||。的！运算符的优先级最高，其次是&&，最后是||运算符。让我们来看看它是什么样子的。

1.  ！真与假
2.  假&&假=假
3.  真||假=真

这次我们来真的。遵循运算符优先级很重要。如果我们想达到我们在第一个例子中达到的结果，我们必须使用括号。

```
<?phpvar_dump( (true || !true) && false );?>
```

当我们使用括号时，我们是在告诉 PHP 先做那个操作。这次我们错了。

需要注意的一点是&&操作符。&&运算符的工作方式是它看左边的表达式，如果是 false，它甚至不会看右边的表达式。PHP 程序员不常这样做，但我见过其他编程语言的程序员利用这一点，有时对他们不利。如果您使用 JavaScript 的 React 库进行过任何类型的编程，您会很快注意到程序员喜欢利用这一点。如果左边的表达式计算结果为 true，则执行右边的一些内容。这在 React 中被称为[条件渲染](https://reactjs.org/docs/conditional-rendering.html)。

要获得运算符及其优先级的完整列表，请查看 PHP 上的[列表。](https://www.php.net/manual/en/language.errors.php7.php)

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/8ea4f8702af7630c65e522e070879790.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。