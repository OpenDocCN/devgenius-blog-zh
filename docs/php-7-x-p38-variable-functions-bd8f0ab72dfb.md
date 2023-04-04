# PHP — P38:可变函数

> 原文：<https://blog.devgenius.io/php-7-x-p38-variable-functions-bd8f0ab72dfb?source=collection_archive---------2----------------------->

![](img/a9c8e7d53e296864083f3e11cf5b17fd.png)

PHP 支持可变函数的概念。这意味着你可以在变量后面加上括号，并把它作为函数调用。虽然不经常使用，但当您希望根据存储在变量内部的值动态调用函数时，它会非常有用。让我们创建一些例子，看看这是非常简单的。

我们将首先创建两个函数:*斯巴鲁()*和*日产()*。这两个函数将*回声*出不同的字符串。然后我们将创建一个 *$car* 变量，并为其分配一个字符串，该字符串匹配其中一个函数名:Subaru*或 Nissan*。*为了调用这个函数，我们将在 *$car()后面附加一对括号。**

让我们更详细地看一下这个例子。

1.  PHP 看到了对 *subaru()* 和 *nissan()* 函数的声明。
2.  在第 12 行，值*斯巴鲁*被分配给变量 *$car。*
3.  在第 13 行，一对括号被附加到变量 *$car* 之后。PHP 看到这一点，把 *$car()* 换成 *subaru()* 。
4.  这调用了 *subaru()* 函数，*回显*出“你得到一个 STi。”
5.  PHP 移到第 15 行，将*日产*赋给*$汽车*变量。
6.  在第 16 行，一对括号被附加到变量 *$car* 之后。PHP 看到这一点，将 *$car()* 替换为 *nissan()* 。
7.  这调用了 *nissan()* 函数，*响应*出“你得到了一个地平线”

虽然这是一个简单的概念，但不应该经常使用，因为它会导致代码难以阅读，并且在某些情况下还会导致安全问题，这超出了本文的范围。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/895c1082888d2b1f56e7be69eb302e19.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)