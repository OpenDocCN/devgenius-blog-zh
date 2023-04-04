# PHP — P56:方法覆盖

> 原文：<https://blog.devgenius.io/php-p56-method-overriding-6ae3784b42c9?source=collection_archive---------4----------------------->

![](img/b3d886514a206434ca77013d3c842b89.png)

PHP 面向对象的原则允许方法重载和方法覆盖。你可能听说过这两个术语，但不知道它们的意思，所以我想帮助你区分它们。

方法重载是指有两个同名但参数数量不同的方法。在像 Java 这样的编程语言中，这很容易实现。您创建了两个同名的方法，并为它们提供了不同数量的参数。

*   面积(直径)
*   面积(长度、宽度)

在上面的例子中，我们有不同数量的参数来计算圆的面积和矩形的面积。

在 PHP 中，你必须使用神奇的函数 *__call()* ，这是另一篇文章的主题。我们在这里讨论方法覆盖。

如果您一直在阅读本系列文章，您可能会记得我们用 bark()方法创建了一个 Dog 类。我们继承了 GermanShepherd 类中的 Dog 类，通过关联，我们继承了所有的方法和属性，包括 bark()方法。

我们希望我们的 GermanShepherd bark 与众不同，所以我们在 GermanShepherd 类中创建了 bark()方法，该 bark()方法覆盖了从 Dog 类继承的 bark()方法。

要重写继承的方法，只需确保方法名和参数数量完全相同。

[](/php-p55-file-separation-c3e848295e8d) [## PHP — P55:文件分离

### 描述

blog.devgenius.io](/php-p55-file-separation-c3e848295e8d) 

从我们离开的地方继续，我们将每个类都分离到它自己的单独文件中。我们将创建一个名为 MethodOverriding.php 的新文件，来展示我们的 bark()方法。

对 bark()方法的调用输出“大声吠叫”，因为这是我们在 GermanShepherd 类的 bark()方法中指定的输出。

作为复习，我们的 GermanShepherd 和 Dog 类包含以下方法。

让我们重写一些其他的方法，比如 walk()。为了使它更有趣，我们还将 walk()方法添加到我们的哺乳动物类中。这样，Dog 类从哺乳动物继承并覆盖哺乳动物 walk()方法，GermanShepherd 类从 Dog 继承并覆盖 Dog walk()方法。但是一步一步来。

首先，让我们只将 walk()方法添加到我们的哺乳动物类中，并从我们的 Dog 类中注释掉 walk()方法。为了更容易可视化，我将所有代码放在一个文件中，但是您应该将文件分开。

当我们实例化我们的 GermanShepherd 类并调用 walk()时，我们得到*我是一个行走的哺乳动物*。接下来，让我们取消 Dog 类中 walk()方法的注释，以便它覆盖哺乳动物 walk()方法。

如果我们再次调用 walk()方法，我们得到*我是一只走狗*。这是因为哺乳动物 walk()方法被覆盖了。

最后，让我们在 GermanShepherd 类中创建一个 walk()方法，该方法返回字符串*I am a walking german shepherd。*

调用我们的 GermanShepherd walk()方法现在输出*我是一只行走的德国牧羊犬。*german shepherd walk()方法覆盖了 Dog walk()方法，Dog walk()方法覆盖了哺乳动物 walk()方法。

这只适用于继承的方法，即公共的和受保护的方法。私有方法不会被继承，所以不会被重写。为了使重写有效，父类和子类之间的公共或受保护可见性修饰符必须保持完全相同。

很简单。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/8ca961a9fa512283773b14412999c16c.png)

Dino Cajic 目前是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 负责人。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。