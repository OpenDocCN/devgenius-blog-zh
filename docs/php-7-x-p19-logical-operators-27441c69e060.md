# PHP — P19:逻辑运算符

> 原文：<https://blog.devgenius.io/php-7-x-p19-logical-operators-27441c69e060?source=collection_archive---------38----------------------->

![](img/31cc4e4c291000759321613e8741e4b1.png)

如果你学过布尔代数或者熟悉逻辑门，逻辑运算符就相当简单。你甚至可能接触过哲学中的逻辑。如果你不熟悉逻辑，这可能会让你有点震惊。只是开玩笑。它只是我们处理中的另一个运算符，计算结果为真或假。

我们看了如何测试一个表达式是否为真，但是我们如何测试多个表达式是否为真呢？用逻辑运算符，就是这样。

PHP 中有一些运算符可以测试两个表达式的真值。

*   AND:仅当左表达式和右表达式的计算结果都为 true 时，才返回 true。
*   &&:与 AND 相同，但优先级不同。
*   ||:如果左表达式或右表达式或两者的计算结果都为 true，则返回 true。
*   OR:与||相同，但优先级不同。
*   XOR:仅当左表达式或右表达式为真，但不同时为真时，返回真。
*   ！:否定。如果为假，则返回真，如果为真，则返回假。

```
<?phpvar_dump( true && true ); // evaluates to true$a = "lowered";
$b = "boosted";var_dump($a == "lowered" AND $b == "boosted"); // true?>
```

在上面的例子中，有了表达式 **true 和 true** ，我们左边是 true，右边是 true。true 和 True 的计算结果为 True。

在第二个例子中，我们评估左边的表达式和右边的表达式。左边的表达式$a == "lowered "，计算结果为 true，因为$a 确实包含字符串" lowered "。右边的表达式也计算为 true，因为$b 确实包含字符串“boosted”既然两边都返回 true，我们就得到 true 和 true。为了使 AND 运算符返回 true，左表达式和右表达式的计算结果都必须为 true。因为他们这样做，它返回真。

我们可以使用求反运算符来获得表达式的相反值。

```
<?php$a = "codeigniter";
$b = "laravel";var_dump( $a == "codeigniter" && $b != "zend" ); // truevar_dump( !true ); // false
var_dump( !false ); // true?>
```

在上面的例子中，左边评估为真，因为$a 确实等于 **codeigniter** 。右边的表达式也计算为真，因为$b 不等于**Zend；这是一个真实的说法。所以真& &真导致真。**

既然我们理解了这个概念，剩下的逻辑操作应该是轻而易举的。

```
<?php
var_dump( true and false); // false
?>
```

只要 AND 语句中有 false，它的计算结果总是 false。下面的示例也计算为 false，因为除了优先级之外，它与 AND 同义。

```
<?php
var_dump( true && false ); // false
?>
```

||运算符称为“包含或”运算符，意思是您可以让任一侧或两侧为真，它将为真。对于||运算符，以下结果将为真:

*   真||假
*   假||真
*   真||真

记住评估为真的陈述是一回事，但是把它形象化是另一回事。如果你上一门课，学校可能会说，“为了上这门高等数学课，你必须完成微积分 3 或数学建模。”如果你完成了这些课程中的任何一门，你就很好。如果你选了这两门课，你也可以选高等数学课。你唯一不能上高等数学课的时候是你没有完成任何一门必修课。

```
<?phpvar_dump( true || false ); // true
var_dump( true || true); // true
var_dump( false || true); // true
var_dump( false || false); // false?>
```

您可以使用 or 运算符代替||，但请记住 OR 和||的优先级不同。

有一个异或运算符 XOR，它不同于我们刚才看到的包含异或运算符。XOR 运算符仅在一边为真而另一边为假时计算为真。

*   真异或假
*   假异或真

让我们也试着想象一下。你可能会被你的服务员问你要汤还是沙拉。你只能选择其中一个，而不能两个都选。如果这是一个 **inclusive-or** 语句，你将能够得到汤和沙拉，但我们正在谈论 **exclusive-or 的**，所以你必须选择一个。

```
<?phpvar_dump(true XOR true); // false
var_dump(true XOR false); // true
var_dump(false XOR true); // true
var_dump(false XOR false); // false?>
```

您可以复合逻辑运算符。请记住，根据优先级，一次只能计算一条语句。你总是会有左右两种表情。

```
<?phpvar_dump( true && (true || false) ); // true?>
```

在上面的例子中，我们首先计算括号内的表达式。True 或 false 计算结果为 true。然后我们可以计算下一个表达式，true 和 true，它产生 true。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/a2baaaf9ceb27a6a5e6c86d096045355.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读迪诺·卡吉克(以及媒体上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。