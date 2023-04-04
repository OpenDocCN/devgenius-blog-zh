# 计算机中浮点数的速成课程，第四部分

> 原文：<https://blog.devgenius.io/a-crash-course-on-floating-point-numbers-in-a-computer-part-iv-40549c7b7a93?source=collection_archive---------5----------------------->

![](img/adbc097d051d6715de537c6f05ec265e.png)

你认为这里从地板到天花板的距离是多少？照片由 [Kvnga](https://unsplash.com/@kvnga?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

浮点数既被广泛使用，又很少被理解。不理解浮点，你迟早会遇到一些奇怪的问题。

没有必要成为 IEEE 754 标准的专家，但是理解浮点数的基础知识对于避免不愉快的意外和不恰当的误用大有帮助。

浮点数系统是一个非常复杂的话题，即使是一篇关于它的基础知识的文章也会很长。这是最后一部分，我保证。任何更多的东西都将是超出速成班范围的高级材料。

在[第一部分](https://medium.com/dev-genius/a-crash-course-on-floating-point-numbers-in-a-computer-part-i-a7a70f67a27)中，我解释了符号位、指数位和尾数位，浮点中“负”零的存在，以及符号、指数和尾数的什么组合代表无穷大和“不是一个数”(NaN)。

我还解释了“正常”和“次正常”(或“反正常”)数字之间的区别。

在第二部分的[中，我解释了一点关于浮点运算的工作原理，以及浮点运算的优势和劣势。](https://medium.com/dev-genius/a-crash-course-on-floating-point-numbers-in-a-computer-part-ii-6fca16a0a321)

如果这些概念对你来说听起来很陌生，我强烈建议你在阅读这一部分之前先阅读第一部分和第二部分。如果你没有读过第三部分，这部分可能有意义，也可能没有意义。

第二部分应该以浮点如何在主要的计算机编程语言中可用来结束。但是我把这些内容分成了第三部分，即使是第三部分也很长。

因此，我将第三部分[限制为主要编程语言中浮点数据类型的概述以及不同位宽的整数和浮点数据类型之间的转换。](https://medium.com/dev-genius/a-crash-course-on-floating-point-numbers-in-a-computer-part-iii-624f43a100aa)

这是讨论如何实际使用浮点值的必要背景，也是这最后一部分的内容。

# 对浮点值进行操作

如果一种编程语言有浮点原语类型，你可以指望它有熟悉的基本算术运算符(`+`、`-`、`*`和`/`)和运算符优先级的一般规则。

正如我们在第二部分看到的，只用加法、乘法和除法就可以得到平方根函数。

科学计算器中的任何函数(对数、三角函数等)。)我们可能想要的，通常可以从基本操作中补上。有时我们可能还需要位运算。

但是通常我们没有必要在它们上面重新发明轮子。在这个速成课程中，重新发明用来说明浮点数系统的一些复杂性。

Java 有`Math`和`StrictMath`类，它们提供了与根和幂、三角函数和其他函数相关的函数。有些语言，如 Pascal，提供这些函数作为语言的内置保留关键字。

为了在 Java 中模拟这一点，您可以为`Math.*`或`StrictMath.*`放入一个静态导入，这样就不用考虑这些函数不是 Java 语言的保留关键字这一事实。

如果没有这样的导入，我们可以使用像`floor`、`sqrt`、`tan`等标识符。，用于我们在 Java 中想要的任何东西(但是我们最好将它们用于执行计算的函数，比如那些在`Math`和`StrictMath`类中的函数)。

例如，要得到一个正数的平方根`x`，我们可以调用`Math.sqrt(x)`或`StrictMath.sqrt(x)`。为了得到`x`的正弦，我们调用`Math.sin(x)`或者`StrictMath.sin(x)`。

`Math`和`StrictMath`之间最重要的区别在于，后者需要根据可自由分发的数学库来实现它的函数，因此这些函数无论运行在什么系统上都给出相同的结果。

然而，如果你的任务是编写`Math`，你可以选择将大部分函数通过管道传递给`StrictMath`。事实上，甲骨文 `[Math](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html)`的[官方文档中写道](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html)

> 默认情况下，许多`Math`方法只是简单地调用`StrictMath`中的等价方法来实现。鼓励代码生成器使用平台特定的本地库或微处理器指令，以提供更高性能的`Math`方法实现。

根据您的 Java 安装,`StrictMath`的一些功能可能会被标记为`strictfp`。事实上，这些是我能想到的 Java 开发工具包中唯一的`strictfp`例子。

JavaScript 有一个`Math`对象，它有很多与 Java 的`Math`类相同的功能，这是对 Java 的肤浅认可之一。如果您没有定义对象，也没有从第三方库中导入对象，那么就没有`StrictMath`对象。

Floor 和 ceiling 可能是 Java 的`Math`和`StrictMath`类提供的最重要的“杂项”函数。给定一个“实”数 *x* ，floor( *x* )是最大整数 *n* ，使得 *n* ≤ *x* ，ceiling( *x* )是最小整数 *n* ，使得 *n* ≥ *x* 。

例如，下限(12.75) = 12，上限(12.75) = 13。对于负数，注意方向要正确:floor(12.75)= 13，而不是 12；上限(12.75)= 12，而不是 13。

如果 *x* 是正的但不是次正规的，获得 floor( *x* )的一种方法是根据指数位移动掩码 FFF0 0000 0000 0000(确保填充左边的位)，然后与 *x* 的浮点位模式进行按位 AND 运算，从而消除小数部分。

你怀疑吗？我是。这正是那种非常适合测试驱动开发的复杂事物。在通过了对正的正常数字的测试后，我得到了这个:

这不会给出正确的结果负数，也不会给出任何符号的次正规数。但我向你保证它适用于正的正常数字。

如果我们想让我们的`floor()`给出和`Math.floor()`一样的结果，我们需要确保负零不会变成正零。好吧，我把剩下的部分作为额外的学分练习留给你(并不是说我会给这个速成课程打分)。

我现在重申我在第二部分写的:浮点值的乘除比加减法更可靠。第二部分中提到的臭名昭著的 0.1 + 0.2 ≠ 3.0 / 10 就是一个很好的例子。

但是所有的浮点运算都会损失精度。floor 函数可以突出浮点不精确性的例子。我在 PHP 中研究浮点数的时候发现了一个非常好的例子:floor((0.1 + 0.7) × 10)。

很明显 0.1 + 0.7 = 0.8，所以很明显 0.8 × 10 = 8.0，因此 floor(8.0) = 8。但是，即使您以前没有见过这个例子，您也可以预测，这不是使用浮点得到的结果(除非您使用的是 C#或 Python 中可用的 decimal 类型)。

回想一下第二部分，0.1 不能用任何 IEEE 754 浮点格式精确表示。我们看到 0.1 = 1/10 = 1/2 × 1/5，1/2 可以精确表示，1/5 不能。

由此可见，7/10 也不能被精确地表示出来。在你的 Web 浏览器的 JavaScript 控制台中试试这个(在 Firefox for Windows 中，F12，然后选择控制台选项卡):0.1 + 0.7。结果将是 0.7999999999999999。

那么乘以 10 就是 7.999999999999999。综上所述，`Math.floor((0.1 + 0.7) * 10)`给出的结果是 7，而不是 8。

我看到过这样的建议，当把浮点数加起来时，最好按大小分组。例如，如果您需要将一组混在稍大的数字中的潜在的低于正常值的数字相加，最好先将较小的数字相加，然后再将较大的数字相加。

这在 32 位浮点中似乎是一个比 64 位浮点更大的问题，但是无论如何都要记住。

此外，减法似乎有一些问题，这些问题并不完全是你所期望的相反的加法，需要对被减数和减数进行某种“按摩”。

这对比较浮点数有重要的影响。这反过来会影响浮点数的排序列表和返回浮点值的单元测试函数。

大多数读到这里的人本能地知道如何在头脑中比较数字。比如我问你 7 < 8 是不是真的陈述。是的，这是真的，你不需要任何特定的算法来做出决定。

现在我问你 7/8 < 8/7 是否成立。您可能需要暂停一秒钟来进行推理，但是您应该能够确认这确实是真的，而不需要任何特殊的算法。

对于计算机来说，通常的比较算法是这样的:判断 *a* < *b* ， *a* = *b* 或 *a* > *b* ，计算*a*—*b*。如果*a*b 为负，那么*a*b。如果*a*b= 0，那么 *a* = *b* 。如果*a*—*b*为正，则*a*>b。

确定 7/8< 8/7 is true by that algorithm, a computer would calculate 7/8 − 8/7 = −15/56\. Since that is a negative number, it is indeed the case that 7/8 < 8/7.

If you’ve ever written a 【 class that is 【 , the *a*-*b*算法是否就是你实现`compareTo()`的方式。

这个特殊的例子同样适用于 64 位浮点数。为了验证 0.875 < 1.1428571428571428(这对你我来说似乎是显而易见的)，计算机会计算出 0.8751.144285714857 = 0.26785757，从而确认 0.875 确实< 1.14875。1467

但是在次正规数中，NaN 总是不等于任何其他浮点值(包括它自己)和浮点的其他几个微妙之处，并且复杂性非常高。

以我的经验来看，`Fraction`类是一个很好的工具，可以计算有理数，可能是也可能不是整数，同时避开了 IEEE 754 浮点数的麻烦。

一个`Fraction`实例的分子和分母被保存为整数(或者“大整数”，如果需要的话)，并且提供了一个函数将它们转换为浮点数，将前者除以后者并返回近似的浮点值。例如，15/56 约为 0.2678571428571428。

然后，如果我需要那个浮点值，我写一个函数调用。否则，我就使用`Fraction`的基于整数的算法，而不用担心精度损失，或者出现奇怪的浮点值。

一个`Fraction`对象应该是不可变的，它的构造函数应该确保分母总是一个正整数，如果分母参数是负的，就将分子参数乘以 1(当然也拒绝将 0 作为分母参数)。

这样，比较函数只需要检查差的分子的符号。在 7/8 < 8/7 的例子中，15 为负。浮点不能精确表示 1/56 也没关系。

按值对`Fraction`实例进行排序比对浮点值进行排序容易得多。这几乎和对整数排序一样简单。

使用 Scala，对整数列表进行排序非常容易。如果你有一个本地的 Scala REPL，试着这样做:

```
scala> List(5, -2, -3, -3, 1, 4, 7)
res5: List[Int] = List(5, -2, -3, -3, 1, 4, 7)scala> res5.sorted
res6: List[Int] = List(-3, -3, -2, 1, 4, 5, 7)
```

我决定不使用这些要点，我认为更简单的格式更可能符合你在 REPL 看到的。

容易得很。现在使用`toDouble()`函数将未排序的`res5`(或者在您的 REPL 会话中出现的任何东西)转换成一个 64 位浮点数列表。

```
scala> res5.map(_.toDouble)
res7: List[Double] = List(5.0, -2.0, -3.0, -3.0, 1.0, 4.0, 7.0)
```

当我们试图对`res7`进行分类时，一个令人不快的惊喜等待着我们:

```
scala> res7.sorted
            ^
       warning: object DeprecatedDoubleOrdering in object Ordering is deprecated (since 2.13.0): There are multiple ways to order Doubles (Ordering.Double.TotalOrdering, Ordering.Double.IeeeOrdering). Specify one by using a local import, assigning an implicit val, or passing it explicitly. See their documentation for details.
res8: List[Double] = List(-3.0, -3.0, -2.0, 1.0, 4.0, 5.0, 7.0)
```

它仍然对列表进行排序，但是如果我想对更多的浮点数列表进行排序，我不想一次又一次地收到这个警告。让我们试试`IeeeOrdering`，看看除了压制警告之外还有什么不同。

```
scala> import scala.math.Ordering.Double.IeeeOrdering
import scala.math.Ordering.Double.IeeeOrderingscala> res7.sorted
res9: List[Double] = List(-3.0, -3.0, -2.0, 1.0, 4.0, 5.0, 7.0)scala> res8 == res9
res10: Boolean = true
```

对于一个`List[Double]`，`sorted()`函数接受一个`Ordering[Double]`参数，但是随着`IeeeOrdering`的导入，它变成了隐式参数，我们不需要显式地声明它。

这意味着我们仍然可以使用`TotalOrdering`来代替，如果我们愿意，我们只需要显式地指定它。

```
scala> res7.sorted(scala.math.Ordering.Double.TotalOrdering)
res11: List[Double] = List(-3.0, -3.0, -2.0, 1.0, 4.0, 5.0, 7.0)scala> res8 == res11
res12: Boolean = true
```

因此，`TotalOrdering`给出了与`IeeeOrdering`完全相同的结果，与我们第一次尝试对这些数字进行排序相比，唯一的不同就是没有出现反对警告。

从相关文档中我可以看出，只有当我们的浮点数列表包含 nan 时，这里的选择才会有所不同。

还记得在第一部分中，在 64 位浮点中，有数万亿种方法来表示 NaN，但它们都显示为“NaN”请记住，在第三部分中，Java 倾向于将所有这些不同的 NaN 值合并成一个规范的“正”NaN。

要是有什么方法可以给这些人贴上标签就好了……事实上，确实有。我们可以创建一个`LabeledDouble`类，它将从`long`原语派生的`double`原语与`String`标签配对。

鉴于 Java 所谓的冗长，对于速成班来说，创建一个新类的前景似乎太过繁重。但是在 Scala 中，一个类可以很短，很容易放入一个 Gist 中:

它可以很容易地粘贴到您的本地 Scala REPL 中，看起来应该是这样的:

```
scala> class LabeledDouble(val bitPattern: Long, 
     |                     val label: String){
     | val number = java.lang.Double.longBitsToDouble(bitPattern)
     | override def toString: String = this.label + "(" +
     |                                 this.number + ")"
     | }
defined class LabeledDouble
```

(如果您从该文件复制并粘贴，请确保删除“|”字符)。

现在我们可以列出一个`LabeledDouble`实例的列表。

```
scala> res5.map(n => new LabeledDouble((0x7FF8L << 48) + 16 + n, "Specific NaN #" + n))
res13: List[LabeledDouble] = List(Specific NaN #5(NaN), Specific NaN #-2(NaN), Specific NaN #-3(NaN), Specific NaN #-3(NaN), Specific NaN #1(NaN), Specific NaN #4(NaN), Specific NaN #7(NaN))
```

通过取位模式 7FF8，将其左移 48 位，并添加接近 0 的整数，我们获得了几个不同 nan 的位模式。

```
scala> res5.map(n => new LabeledDouble((0x7FF8L << 48) + 16 + n, "Specific NaN #" + n))
res13: List[LabeledDouble] = List(Specific NaN #5(NaN), Specific NaN #-2(NaN), Specific NaN #-3(NaN), Specific NaN #-3(NaN), Specific NaN #1(NaN), Specific NaN #4(NaN), Specific NaN #7(NaN))scala> res13.sortBy(_.number)
res14: List[LabeledDouble] = List(Specific NaN #5(NaN), Specific NaN #-2(NaN), Specific NaN #-3(NaN), Specific NaN #-3(NaN), Specific NaN #1(NaN), Specific NaN #4(NaN), Specific NaN #7(NaN))scala> res13 == res14
res15: Boolean = truescala> res13.sortBy(_.number)(scala.math.Ordering.Double.TotalOrdering)
res16: List[LabeledDouble] = List(Specific NaN #5(NaN), Specific NaN #-2(NaN), Specific NaN #-3(NaN), Specific NaN #-3(NaN), Specific NaN #1(NaN), Specific NaN #4(NaN), Specific NaN #7(NaN))scala> res13 == res16
res17: Boolean = true
```

好吧，我不知道。我觉得这很有趣，但是因为离题太多，我把它作为另一个额外的收获:构造一个浮点数列表的例子，这样选择`TotalOrdering`或`IeeeOrdering`就有所不同。请在评论中发表你的解决方案。

这里一个更普遍的问题是，当一个浮点数与期望值相差一些可接受的小偏差时。

我不确定是哪个版本的 JUnit 弃用了用于`double`的双参数`assertEquals()`，但是我可以告诉你，从 JUnit 4.12 开始，使用它会导致测试失败，就像你写了`fail()`一样，不管数字是否相等。

要点没有显示的是，在您的集成开发环境中(例如 NetBeans)，应该删除“`assertEquals`”。

这将失败，并显示消息“使用 assertEquals(expected，actual，delta)来比较浮点数”比较两个`float`数字也是如此。

“差值”是预期结果的最大容许公差或偏差。一些 Java 程序员似乎认为这很讨厌，所以他们选择了 0.0 的 delta。

也许这有时对你有用。但它迟早会咬你。还记得在第二部分中的例子吗，我们如何在 64 位浮点中得到√8 的两种不同的近似值:2.82842712474619 和 2.82474603。

将这两个数字平方，我们得到 7.999999999999998 和 8.000000000002。这两个数字都与 8 相差 0.000000000002，只是方向相反。

很明显，0.0000000000002 接近于 0，但它并不完全是 0.0。因此，对预期结果偏差零容忍的测试很可能会失败，因为方差很小，我们可能不在乎它。

对于平方根函数来说，只有当它使用一个可以精确表示为浮点数的有理数平方根时，才可以通过公差为 0.0 的测试。

这可能会使使用伪随机数发生器的测试过于复杂。实际上，在这种特殊情况下，这可能并不十分困难…

尽管如此，作为容差，最好是一个小的正数，不一定低于正常值。例如:

假设测试选择了 31，被测试的函数给出的平方根是 19 . 7667 . 567676767671

将这个数乘以它本身，你得到 391.5802590948274，加上 0.00001 的容差，这已经足够好了。

顺便说一下，`Math.sqrt(391.5802590948273)`给出的是 19.7838697556795，它乘以自身给出的是 391.46477777772。因此，如果公差不合理，如 0.0，也会失败。

如果您在一个测试类中有足够多的这样的测试，您可能想要声明一个常量，并将其命名为类似于`DELTA`或`TEST_DELTA`的东西。

你们中的一些人可能真的想写返回负零和特定 NaNs 的函数，原因我只能猜测。

因为负零在技术上等于正零，所以 JUnit 对浮点数的`assertEquals()`可能会导致错误通过。您可能需要测试位模式。

在 Java 中工作，记住规范的 NaN。JUnit 的`assertEquals()`经过`Double.compare()`，用规范的 NaN 来比较 NaN 值，这样所有 NaN 都是相等的。同样，您可能需要测试位模式。

# 概括起来

浮点运算在几乎所有主流编程语言中都非常方便。

然而，浮点运算的整个 IEEE 754 系统具有很大的复杂性和很大的精度意外损失的可能性。

我提到过 C#和 Python 都有十进制浮点类型，这可能比 IEEE 754 浮点有更少的陷阱。可能别人会写一篇解释十进制浮点基础知识的文章(我想链接中等文章)。

任何浮点类型都不能精确地表示无理数。然而，浮点类型也不能表示许多看起来在范围内的有理数。

因此，这些数字必须是近似值。比如有理数 1/ *p* ，其中 *p* 为奇素数，在 IEEE 754 浮点中无法精确表示。也不能用十进制浮点格式表示，除非*p*= 5 或 5。

除了很多有用的有理数，IEEE 754 浮点数还可以表示负无穷大、正无穷大和非一数(NaN)。

像平方根这样的“高级”算术运算可以通过像加法和乘法这样的“基本”算术运算获得。

一般来说，乘法和除法是比加法和减法更可靠的运算。这一点，以及负零的存在，使得浮点数的比较变得非常复杂。

因此，浮点必须小心使用，返回浮点数的函数需要在测试中内置合理的小非零容差来测试。