# Unity Dev 的第 127 天:Singletons— Unity/C#！

> 原文：<https://blog.devgenius.io/day-127-of-unity-dev-singletons-unity-c-781e8c34adf0?source=collection_archive---------15----------------------->

**目标:**回顾什么是单体设计模式，以及一些利弊。

注意:本文假设您对属性有所了解。如果你感兴趣的话，这是我写的关于[房产](https://medium.com/dev-genius/day-115-of-game-dev-how-to-use-properties-c-7409cac5fa11)的文章。你不一定需要知道什么是属性来理解单例设计模式，但是这将会有很大的帮助。

![](img/9e67c30efc16e9861cecb88389db02a5.png)

[形象学分](https://www.educative.io/answers/how-to-implement-the-singleton-pattern-in-c-sharp)

在开发中，单例设计模式倾向于大量应用于管理器类。

什么是独生子女？单例是创建类的单个实例的一种方式。一个传统的类被创建，它实际上只是一个蓝图。这个类还没有被真正使用过。然后，您必须创建该类的一个实例(对象),这意味着要使用该蓝图。

房子的蓝图是一个类，实际的房子是该类的一个实例。

当你创建一个单例类时，它自动成为该类唯一存在的实例，并且既是蓝图也是蓝图的实际实例。

需要被许多不同脚本访问的类，像 GameManager、SpawnManager 或 UIManager 类一样只需要存在一次，通常适合作为单例使用。使类成为单例类会使访问它们变得容易得多，因为你不必获得一个类引用来与单例类交互。

你可能会问，“既然单例类和静态类都是所述类的一个实例，那么它们之间有什么区别”。

![](img/017e7c4a9f603a7d673f34d3f2793413.png)

[学分](http://net-informations.com/faq/netfaq/singlestatic.htm)

![](img/1bccae08c0cae65e5cdaf88a3f170efa.png)

[学分](https://stackoverflow.com/questions/2765060/why-use-a-singleton-instead-of-static-methods)

![](img/5de3ef7ab6512eae40ccf50a61f1d81e.png)

[学分](https://stackoverflow.com/questions/519520/difference-between-static-class-and-singleton-pattern)

在我们继续创建单例类之前，我想提一下，当你创建一个静态类时，通常只需输入 static 关键字。

![](img/f663f34f512a11a4a0cf69e35c2a9566.png)

这对单例类来说是不一样的。静态类有一个特殊的关键字来设置它，但是单例类没有。相反，singleton 需要几个步骤，并且没有可以应用于该类的特定关键字。

我们如何创建一个单例类？我将做一些设置并创建一个游戏管理器脚本和一个玩家脚本来测试对游戏管理器的访问。

![](img/b3ee5636108b6684e455c3b26b123e71.png)

我们将从游戏管理器开始。

![](img/ea6113263dea55af0461ddc8a4e47394.png)

这段代码让 GameManager 遵循了 singleton 设计模式。我们来分解一下。

我想指出的第一件事是，是的，在这个单例类中有静态变量。类本身并不是静态的，这就是我们在本文前面提到的与继承相关的优点，以及与静态类相比的更多优点。

相反，我们使用静态属性来创建一个更灵活的“静态”类，称为 singleton。但是怎么做呢？让我们从头开始。

首先，我们有我们的游戏经理类。记住，这个类现在是我们可以使用的类型。从技术上讲，现在它就像一个普通的类。

![](img/9a4844330373f57e8eff0d4f77d04822.png)

然后我们有了 GameManager 类型的私有变量(这个类)。请记住，命名变量实例是一种常见的做法。

![](img/5c95d641763b571dc5da611ccef81cad.png)

这与静态类有一点不同。前面我说过，在某种程度上，静态类基本上既是蓝图，也是该类的实际实例。相反，我们在这里创建一个单独的 GameManager 类的实例，并使其成为静态的，这样就只有一个实例存在。从技术上来说，你可以创建更多的实例，实际上并不局限于只创建一个实例，但是这里的要点是创建一个并且只使用一个。

现在我们需要一个公共属性来保护对上面创建的私有 GameManager 实例的访问。

![](img/16b692cd965f025e56c9cd303e521da6.png)

我们将只在属性上放置一个 getter，以便可以访问它。(确保该属性是静态的，并且我们只需要该现有属性的一个实例)

![](img/924b6764efe6c32a0f4edb2ec544fd45.png)

在 getter 中，如果实例不存在，我们要么返回私有变量，要么记录 GameManager 为空。

![](img/503a23780f899bb3dbf363e7f18da75b.png)

最后，在 Awake 中，我们确保将私有实例变量设置为**这个**的含义，**这个脚本**。如果我们不这样做，那么 _instance 变量就不会被赋值。它将为空。所以我们将 GameManager _instance 分配给这个 GameManager 脚本。当你习惯了静态类时，这样做可能会感觉很奇怪，因为静态类会自动这样做。当然，这里我们试图避免静态类的缺点，同时获得静态类的优点。

这就是我们将这个类设置为单例所需的全部内容。我将添加一个测试方法，这样我们就可以从 Player 类中调用它，并确保它能够工作。

![](img/4e0448cd80f87104cd004dbefb53476d.png)

正如你在这里看到的，在 Player 类中，我们不需要通过 inspector 或者使用 GetComponent 之类的方法来引用 GameManager 类。我们可以很容易地访问它，这对于保持你的代码模块化非常有价值。

![](img/070730b93f7e6130b005ddc615f89217.png)

在我将玩家和游戏管理器脚本连接到场景中的一个空游戏对象并按下 play 之后，它成功地从玩家脚本中打印出游戏管理器脚本中的测试方法。

![](img/3f7132a1a5033a87f9231ee73c0634e5.png)

我想谈谈单身族提供的另一种可能性/好处。这就是所谓的懒惰实例化。

![](img/4a3046bfc2820e1ff5706cbec2d90897.png)

回到我们的 GameManager 脚本，如果 _instance 为空，我们会记录错误。这里还有另一个选择需要记住。

如果还没有游戏管理器，我们可以实时创建一个。

![](img/dcebe0d0ef13de78a35f8e12bebdf480.png)

上面的代码替换了错误日志，而是创建了一个新的游戏对象，并向其中添加了 GameManager 脚本。

所以现在如果我把游戏管理器从我的游戏中移除并点击 play，一个游戏管理器将会被创建并且不会出错，因为它不是空的。

![](img/ebda7b59e825c08ede3d836484bc0697.png)![](img/e03afc8d97e30bac927e393da0c09ba5.png)

在大多数情况下，最好的做法是确保已经创建了对象，并用调试日志对其进行 null 检查，但这只是另一个要记住的选项，因为一些罕见的情况可能真的会从中受益。

***如有任何问题或想法，欢迎评论。***