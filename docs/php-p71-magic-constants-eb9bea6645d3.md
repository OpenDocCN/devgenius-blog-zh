# PHP — P71:魔法常数

> 原文：<https://blog.devgenius.io/php-p71-magic-constants-eb9bea6645d3?source=collection_archive---------8----------------------->

![](img/b00764b305a59a28c6c25ae8a7faa260.png)

我们将探索 PHP 的神奇常数。

*   __LINE__ 显示文件的当前行号。
*   __FILE__ 显示文件的完整路径和文件名。
*   __DIR__ 显示文件的目录。
*   __FUNCTION__ 显示函数名，或匿名函数的{closure}。
*   __CLASS__ 显示类名。类名包括声明它的命名空间。
*   __TRAIT__ 显示特征名称。trait 名称包括声明它的名称空间。
*   __METHOD__ 显示类方法名。
*   __NAMESPACE__ 显示当前名称空间的名称。

回顾:回顾我们上次停止的地方。

[](https://github.com/dinocajic/php-7-youtube-tutorials/tree/master/70%20Subnamespaces) [## PHP-YouTube-教程/70 个子命名空间

### PHP YouTube 教程的代码。为 dinocajic/PHP-YouTube-tutorials 开发做贡献，创建一个…

github.com](https://github.com/dinocajic/php-7-youtube-tutorials/tree/master/70%20Subnamespaces) [](/php-p70-sub-namespaces-11ff9c0f0787) [## PHP — P70:子名称空间

### 如果可以定义名称空间，那么定义子名称空间也没什么不同。我们还将看一下与…有关的课程

blog.devgenius.io](/php-p70-sub-namespaces-11ff9c0f0787) 

魔法常数并不复杂。说明它们的最好方式是通过一个例子。让我们对我们的 *Car* 类中的*get _ year _ make _ and _ model()*方法做一个修改，并开始包含这些神奇的常数，看看它们有什么作用。

我们首先要看的是 *__LINE__* 魔法常数。

你认为我们得到了什么？没错。这是代码中的当前行。在这种情况下，应该是“Line #: 13 ”,因为这是包含 *__LINE__* 魔法常量的代码行。

*__FILE__* 魔术常数返回插入 *__FILE__* 魔术常数的绝对文件路径。对我来说就是*C:/www/YouTube/PHP/71 _ Magic _ Constants/car . PHP*。

*__DIR__* 魔法常数为我们提供了包含 *__DIR__* 魔法常数的文件的目录。它与 *__FILE__* 几乎相同，但不包括文件名本身。

*C:/www/YouTube/PHP/71 _ Magic _ Constants*

*__CLASS__* 返回包含 *__CLASS__* 幻常数的类名，即 *Vehicles\Cars\Car。它给出了以全名为空格的输出，这样你就知道它实际上引用的是哪个类。*

我想你开始明白了。如果我们调用 *__METHOD__* 和 *__ NAMESPACE__* 魔法常数，它们将返回包含这些魔法常数的方法和名称空间，在本例中为*Vehicles \ Cars \ Car::get _ year _ make _ and _ model*和 *Vehicles\Cars* 。

唯一没有被我们关注的是 *__TRAIT__* 。如果我们把这个放在同一个代码中，我们不会得到任何东西，因为这个类不是一个特性。不过，我们也有一些特质，比如 *Engine* 特质，我们可以修改一下它的方法 *check_oil_level* 。

如果我们调用上面的代码，我们得到*车辆/汽车/停车场/发动机*作为输出。

就是这样。我告诉过你，这将是美好而简单的。大多数情况下，您会在日志文件中使用它们，以便快速指向错误。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinochaic/PHP-YouTube-教程

### PHP YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/f8fc58490bc373090a2c3f8382795e26.png)

Dino Cajic 目前是 LSBio(lifetime bio sciences，Inc.) 、[绝对抗体](https://absoluteantibody.com/)、[角蛋白](https://www.kerafast.com/)、[珠峰生物科技](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还是我的自动化系统公司的首席执行官。他拥有超过十年的软件工程经验。他拥有计算机科学学士学位和生物学副学位。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识传播。

您可以通过 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 与他联系，通过 [Instagram](https://instagram.com/think.dino) 跟随他，或者[访问他的博客](https://www.dinocajic.com/)，或者[订阅他的 Medium 出版物](https://dinocajic.medium.com/subscribe)。

[*阅读 Dino Cajic(以及 Medium 上成千上万其他作家)的所有故事。您的会员费直接支持 Dino Cajic 和您阅读的其他作家。你还可以全面了解 Medium 上的所有新闻。*](https://dinocajic.medium.com/membership)