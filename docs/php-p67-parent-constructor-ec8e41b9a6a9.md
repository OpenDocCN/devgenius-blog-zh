# PHP — P67:父构造函数

> 原文：<https://blog.devgenius.io/php-p67-parent-constructor-ec8e41b9a6a9?source=collection_archive---------8----------------------->

![](img/b1742b664e443bd1037dbad0dc72c9da.png)

在过去的几篇文章中，我们已经调用了父构造函数，但并没有深入讨论它的细节。如果子类没有定义构造函数，它就像任何其他方法一样继承父构造函数。如果子类定义了自己的构造函数，父构造函数将被重写。要调用父构造函数，必须从子构造函数调用 *parent::__construct()* 。

*回顾*:回顾之前的文章和代码。

[](/php-p66-object-comparison-a2e84a8e3e13) [## PHP — P66:对象比较

### PHP 允许你用比较和标识操作符来比较对象。使用比较运算符时…

blog.devgenius.io](/php-p66-object-comparison-a2e84a8e3e13) [](https://github.com/dinocajic/php-7-youtube-tutorials/tree/master/66%20Object%20Comparison) [## PHP-YouTube-教程/66 对象比较

### PHP YouTube 教程的代码。为 dinocajic/PHP-YouTube-tutorials 开发做贡献，创建一个…

github.com](https://github.com/dinocajic/php-7-youtube-tutorials/tree/master/66%20Object%20Comparison) 

看看兰博基尼类，它只是一个扩展 Car 的空类。Car 类包含一个构造函数。

我们可以实例化该类并使用父构造函数，而无需在兰博基尼类中指定构造函数。

调用*get _ year _ make _ and _ model()*方法得到 *1999 款兰博基尼 Diablo SV* 。这意味着父构造函数实际上被调用了，它设置了 *$year，$make，*和 *$model* 属性。

如果兰博基尼类有一个构造函数会怎么样？兰博基尼构造函数也将接受 3 个参数:品牌、型号和颜色。这次我们过不了这一年了。

我们现在可以尝试一些测试代码，看看它是否有效。

果不其然，确实如此。你可能会认为这不应该考虑到我们从来没有将年份传递给父构造函数。因为我们调用的是兰博基尼构造函数，所以父构造函数永远不会被调用。

但是，如果我们确实想调用父构造函数并在使用兰博基尼构造函数时设置年份呢？

这次我们将修改构造函数以接受 4 个参数: *$year，$make，$model，*和 *$secret_code* 。为什么 *$secret_code* ？因为它不存在于汽车类内部。该属性将存在于兰博基尼类中。这将使代码更容易理解。

```
public function construct($year, $make, $model, $secret_code) { }
```

我们可以使用 *$this* 关键字设置 *$year、$make* 和 *$model* 属性，或者我们可以使用父构造函数。我们如何从子构造函数内部调用父构造函数？

```
parent::__construct()
```

现在，它在兰博基尼类中设置了 *$secret_code* ，并调用汽车构造函数来设置其他三个属性。

让我们为 *$secret_code* 属性添加 getter/setter 方法，这样我们可以做一些额外的测试。

运行代码给了我们想要的结果: *1999 兰博基尼 Diablo SV 0x4225sdf4。*

最后要注意的是调用父构造函数的位置。虽然其他面向对象的编程语言要求您首先调用父构造函数，但在 PHP 中，您可以在构造函数中随时调用它。下面的代码也可以。

我仍然喜欢立即调用父构造函数，然后在子类构造函数中执行任何额外的代码，以防 PHP 将来改变主意。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/13c037b8c41f96613a044ccb55349a5b.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，[订阅他的博客](https://www.dinocajic.com/)，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。