# PHP — P45:类常量

> 原文：<https://blog.devgenius.io/php-p45-class-constants-4a93c49b917f?source=collection_archive---------20----------------------->

![](img/fa73d9c994c71e19ef4b8865557c9dd9.png)

类常数类似于常规常数，只是它们存在于类中。类常数不能改变，因此命名为常数。它们是使用 **const** 关键字声明的，并且每个类在内存中分配一次，而不是每次实例化对象时分配；这就是当你听到常量存在于类中时的含义。在这个意义上，它们类似于静态变量，但是静态变量是可以修改的。要访问类常量，您需要使用类名和范围解析操作符(双冒号)。

常数没有任何可见性修饰符，如 private、public 或 protected，并且它们不像属性那样在前面有$。举例说明常数的最快方法是用一个例子。让我们建立在前一篇文章中创建的 Dog 和 Car 类的基础上。

[](/php-p44-class-properties-277c7c17b74b) [## PHP — P44:类属性

### 属性，也称为类成员变量、属性和字段，是类的特征。想象一下…

blog.devgenius.io](/php-p44-class-properties-277c7c17b74b) 

如果我们从 Dog 类开始，那么每只狗应该有哪些常量呢？换句话说，什么东西是每只狗之间不变的？我脑海中闪现的几个常量是，每只狗都有一条尾巴和一颗心。所以，让我们把这些加到课堂上。记住，常量被添加在方法之外，但是在类花括号之内。它们以关键字 **const** 开头，通常全部大写，以帮助区别于所有其他关键字。

*const HAS _ HEART = true；
const HAS _ TAIL = true；*

如果我们想快速显示常量的输出，我们不必实例化对象；我们可以使用 resolution 操作符来访问 Dog 类中的常量值，即 Dog::HAS_HEART。

*var_dump(狗::HAS _ HEART)；*

再来看另一个例子，汽车类。为了让汽车在街道上合法行驶，它必须有前灯、尾灯和转向灯。这些是我们将为汽车类定义的常量。

同样，为了定义这些，我们从常量名称后面的 **const** 关键字开始。我认为在这一点上没有必要提及常数的命名约定，因为常数遵循与 PHP 中所有其他标签相同的命名约定，但这里是这样的:有效的常数名称以字母或下划线开头，后跟任意数量的字母、数字或下划线。

*const HAS _ heads = true；
const HAS _ TAIL _ LIGHTS = true；
const HAS _ TURN _ SIGNALS = true；*

我们也不必只是 var_dump 我们的常量值；我们可以用它们来构建一个字符串。让我们将常量值组合到一个三元运算符[中来创建一个小字符串。](/php-7-x-p27-ternary-operator-a625bb7fa311)

*附和“此车辆是”。((车::有 _ 大灯)？“一辆车。”:“不是车。”);*

如果我们正确地定义了我们的常量，上面语句的输出应该是:*车辆是一辆汽车。*

我将留给你们最后一个关于常数的想法。常数可以存储与类中的属性相同的值。关于常数，你只需要知道这些。祝编程愉快。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/64a4991e1fbf164448de5462ff106626.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。