# PHP — P59: Self 关键字

> 原文：<https://blog.devgenius.io/php-p59-self-keyword-ae01f24e73b1?source=collection_archive---------6----------------------->

![](img/65c8a229669de314c36250f71e92dbd2.png)

大多数书都是这样写成的。让我们使用一些关键字，而不需要事先向您解释。我也中了那个圈套。我们在前两篇文章中看到了 *self* 关键字，但并没有详细说明它的含义。

[](/php-p58-static-keyword-da0b92d45b3c) [## PHP — P58:静态关键字

### 静态方法是属于类而不属于对象的方法。

blog.devgenius.io](/php-p58-static-keyword-da0b92d45b3c) 

对于静态类成员，我们不使用 [*$this* 关键字](https://dinocajic.medium.com/php-p47-this-keyword-f7397e560949)，我们使用 *self* 。那和 [*作用域解析操作符*](https://dinocajic.medium.com/php-p57-scope-resolution-operator-19c50ca607d4) 允许我们访问静态属性和方法。与 *$this* 关键字不同， *self* 关键字前面不需要美元符号。将 *$* 推向静态属性， *self::$property* ，而对于带有 *$this* 关键字的非静态访问，将从属性名: *$this- >属性中去掉美元符号。*

要访问一个非静态的属性或方法，您可以将 object 操作符添加到 *$this* 关键字中。要访问类中的静态属性或方法，可以使用 *self* 关键字，后跟范围解析或双冒号操作符。

静态属性和方法可以从静态和非静态方法返回。

*重述。*看看下面这个我们在之前的文章中已经介绍过的类: [GermanShepherd](https://github.com/dinocajic/php-7-youtube-tutorials/blob/master/GermanShepherd.php)

我们已经在我们的德国牧羊人课程中审视了当前的自我。

我们在这里所做的是创建一个名为 *$num_of_dogs_created* 的静态属性，并在它下面创建一个名为*update _ num _ of _ dogs _ created()*的静态方法。这两个都是类成员，要访问它们，不必实例化 GermanShepherd 对象。

为了测试我们的示例，我们将创建一个新文件，并添加与前一篇文章中相同的代码。

我们的 GermanShepherd 类被包含和实例化。为了调用静态成员，我们不需要实例化它，但是一旦我们这样做了，我们在构造函数中有一个调用来更新创建的狗的数量。

为了从我们的构造函数中访问*update _ num _ of _ dogs _ created()*，我们必须使用 *self* 关键字，因为它是一个静态方法。我们不能用*$这个*。我们必须使用 *self* 后跟范围解析操作符，最后是静态方法名。

静态方法增加静态属性 *$num_of_dogs_created* ，再次通过将 *self* 附加到属性名:*self::$ num _ of _ dogs _ created ++上；*

我们再创建一个*非静态*方法:*get _ num _ of _ dogs _ created _ plus _ two()*。它只是一些随机的东西；不要过度解读:)。这个方法所做的就是，你猜对了，返回创建的狗的数量并加 2。

这只是为了说明您可以调用返回静态值的非静态方法。让我们打电话给它，看看我们得到了什么。

输出应该如下所示:

*创建的实例数:1
再次更新:2
创建的狗数+ 2: 4*

遍历代码:

*   我们调用一个静态属性， *$num_of_dogs_created，*，它返回 1。
*   然后我们调用一个静态方法，*update _ num _ of _ dogs _ created()*，它将静态属性增加到 2。
*   静态属性 *$num_of_dogs_created* 被再次调用以显示 2。
*   最后，我们调用一个实例方法，*get _ num _ of _ dogs _ created _ plus _ two()*，它返回静态属性 *$num_of_dogs_created* 的值，增加 2。

说到*自我*关键词就这么多了。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/a62ee95e38ac0fdea2bbee35de346598.png)

迪诺·卡希奇目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，[订阅他的博客](https://www.dinocajic.com/)，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。