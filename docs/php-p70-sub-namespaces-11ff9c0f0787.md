# PHP — P70:子名称空间

> 原文：<https://blog.devgenius.io/php-p70-sub-namespaces-11ff9c0f0787?source=collection_archive---------14----------------------->

![](img/33f1b914821752c74494ca47f49d7096.png)

如果可以定义名称空间，那么定义子名称空间也没什么不同。我们还将查看来自不同子命名空间的同名类，以及我们将如何处理那些仍然可能出现的命名冲突。

*回顾*:回顾我们上次停止的地方。唯一的区别是我们将名称空间从 CoolCars 更改为 Vehicles。

[](/php-p69-defining-namespaces-5a8e0a4bfdc0) [## PHP — P69:定义名称空间

### 定义名称空间非常简单。在文件的顶部，就在开始的 PHP 标签之后，名称空间关键字是…

blog.devgenius.io](/php-p69-defining-namespaces-5a8e0a4bfdc0) [](https://github.com/dinocajic/php-7-youtube-tutorials/tree/master/69%20Defining%20Namespaces) [## PHP-YouTube-教程/69 定义名称空间

### PHP YouTube 教程的代码。为 dinocajic/PHP-YouTube-tutorials 开发做贡献，创建一个…

github.com](https://github.com/dinocajic/php-7-youtube-tutorials/tree/master/69%20Defining%20Namespaces) 

如果我们从汽车类开始，我们可以看到它属于*车辆*名称空间。

我们希望这个类成为它自己的子名称空间的一部分，就像*汽车*子名称空间一样。我们如何实现这一目标？通过在*车辆*名称空间后添加\ *汽车*。

你猜怎么着？一切都坏了。为什么？从技术上讲，我们比其他任何东西都要深一个子目录。其他所有代码都存在于*车辆*中，而汽车类存在于*车辆\汽车*中。

现在我们知道了目录结构，我们如何修改代码以使它正常工作呢？我们将使用代码中引用的每个类的绝对路径。例如，由于我们要扩展 Vehicle 类，我们需要给它加上前缀 *Vehicles* 名称空间。

我们需要对代码的剩余部分做同样的修改。既然我们已经实现了引擎和传输特性，它们也需要修改。

如果我们不使用反斜杠，例如 Vehicles\Vehicle，我们会得到一个新的错误:*在\ Vehicles \ Car \ Vehicles \ Vehicle*中未定义的名称空间‘Vehicles’。反斜杠表示从全局名称空间开始。如果我们不包含它，它会追加到当前的名称空间。

完事了吗？没有。还是坏了。我们必须开始深入挖掘，看看汽车类在哪里得到了扩展。那会是在兰博基尼和法拉利级别。

我知道你现在明白了，但是让我们更进一步。我们汽车班用的是发动机和变速器特性。那些是汽车零件，应该放在 *Vehicle\Cars* 下的 *CarParts* 子名称空间中。

我们打破了什么吗？当然了。我们需要修改所有使用这些特征的代码，例如 Car。

我们只是让 PHP 知道如何在我们新定义的虚拟目录系统中找到那些特定的类。

到目前为止，我们还没有修改的另外一个类是驱动程序类。它通过构造函数注入汽车类。不要直接在构造函数中进行修改，记住我们可以在类声明之前使用我们的 *use* 关键字，以避免代码中的歧义。我们还可以在*车辆\汽车\司机*子名称空间中定义我们的司机。

我们还能修改什么吗？我们当然可以。兰博基尼和法拉利是一种车:一种异国情调的车。让我们用子名称空间来明确这一点。

是时候做些测试了。我们可以从我们的兰博基尼类中调用*get _ year _ make _ and _ model()*方法，看看我们是否得到了我们想要的结果。

我们得到了 *1999 款兰博基尼 Murcielago* ，这正是我们所期待的。

如何对名称空间/子名称空间进行分类实际上取决于您。

最后一件要解决的事情是命名冲突，这是名称空间/子名称空间真正闪光的地方。如果我们想创建另一个兰博基尼类，我们可以。这可能是我们的山寨兰博基尼。我们想给它起一个完全一样的名字，而不是兰博基尼山寨版。

我们如何做到这一点？我们将把它放到自己的名称空间中。所有外来的仿冒品都将进入*车辆/汽车/外来车/仿冒品*名称空间。

我们来测试一下。我将方便地使用 *use* 关键字…我们已经破解了代码。PHP 将不知道调用哪个兰博基尼。我们必须明确声明我们想要使用一个兰博基尼类和一个山寨类。怎么会？通过在*后面添加*作为*来使用*。然后我们可以使用我们在 *use* 语句*中定义的任何东西。*

如果不想定义山寨类，就必须在代码本身中附加整个名称空间结构，这样就不会有歧义。这导致代码混乱。

我希望你明白这并不可怕。每当你看到一个巨大的结构，它只是有人在文件的顶部定义名称空间。只是一个虚拟的目录结构。仅此而已。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-YouTube-教程

### PHP YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/c9edb6e92c1f69b60064cbbc5ac74a51.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，[访问他的博客](https://www.dinocajic.com/)，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。