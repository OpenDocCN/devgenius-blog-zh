# 观察者模式的一个例子

> 原文：<https://blog.devgenius.io/1-example-of-observer-pattern-3db3b3d29b86?source=collection_archive---------14----------------------->

## 顶尖的开发人员会在你下次面试时问你这个问题

![](img/92ebde3081a3d7c18c1d38cf513b02db.png)

[产品学校](https://unsplash.com/@productschool?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/celebration?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

如果你是编程新手，很可能你还没有听说过软件设计模式。这些是众所周知的、广泛使用的解决常见问题的方案，软件开发社区通过多年的努力工作、研究和反复试验已经设法解决了这些问题。

最终，顶尖的开发人员会遇到类似的问题。

## 他们没有重新发明轮子

其中最重要的问题是与应用程序的架构有关的问题，如果处理不当，可能会在未来成为一个严重的问题。如果您正在为某个特定的解决方案对未来变化的响应能力而绞尽脑汁，那么您可能正在处理一个可以使用设计模式解决的问题。

毕竟，一个好的软件架构应该足够灵活，能够改变和成长。

实际上有几十种不同的软件设计模式，分为三大类:创造型、结构型和行为型。让我给你一个大图，下面的列表只描述了其中的四个。

## 模型-视图-控制器

MVC 广泛应用于多页面 web 应用程序(MPA ),并建议将代码分解为三个独立的部分:视图、模型和控制器。视图层包括 UI 逻辑——通常是 HTML、CSS 和 JavaScript——而模型负责业务逻辑。控制器充当视图和模型之间的粘合剂。

## 依赖注入

依赖注入用于将类与其依赖项解耦。一个 OOP 类通常有属性——如果你喜欢的话也可以是变量——包含当然需要首先实例化的对象。这些可以在课堂内外创建。事实是，最好在类外实例化一个对象，然后通过构造函数或 setter 方法将其分配给一个属性。

## 工厂方法

工厂方法模式是一种创造性的模式，它通过特殊的工厂方法创建对象，而不是用`new`操作符直接创建对象，从而促进松散耦合。

## 观察者模式

观察者模式是一种行为设计模式，通过这种模式，被称为观察者的多个对象被告知发生在所谓的主体(即它们正在观察的对象)上的事件。观察者模式可能有点难以理解，因为缺少学习的例子。我想我会向你展示一个可以帮助你成为顶尖开发人员的工具，请继续向下滚动页面以了解更多信息。

## 坚持下去，保持耐心！

现有设计模式的列表还在继续——您可能想访问下面的链接以获得更多信息。

[](https://medium.com/@Mahmoud_Zalt/software-design-patterns-simplified-8a72232d52b1) [## 简化的软件设计模式

### 首先让我们从为什么设计模式存在开始！开发人员已经注意到他们似乎正在解决问题…

medium.com](https://medium.com/@Mahmoud_Zalt/software-design-patterns-simplified-8a72232d52b1) [](https://medium.com/swlh/design-pattern-explained-in-five-minutes-4eae439005d6) [## 在五分钟内解释设计模式

### 常见设计问题的常见设计解决方案

medium.com](https://medium.com/swlh/design-pattern-explained-in-five-minutes-4eae439005d6) [](https://refactoring.guru/design-patterns/catalog) [## 设计模式目录

### 按意图、复杂性和流行程度分组的设计模式目录。目录包含所有经典设计…

重构大师](https://refactoring.guru/design-patterns/catalog) 

如果一开始这个话题看起来太复杂，不要气馁。

如果你读了我以前的一篇题为“PHP 多态例子”的文章，你会发现编写你的第一个面向对象的程序并不太容易。我们在这里谈论的是隐性知识，这与学习演奏乐器或学习一门新语言非常相似。

[](https://medium.com/codex/php-examples-of-polymorphism-ab70d58cf41a) [## 多态性的 PHP 示例

### 允许编写更少的代码

medium.com](https://medium.com/codex/php-examples-of-polymorphism-ab70d58cf41a) 

## 这里有一个 PHP 中观察者模式的例子

`[Chess\Board](https://php-chess.readthedocs.io/en/latest/board/)`是一个棋盘表示，允许以可移植游戏符号(PGN)格式下棋。此外，它也是在其上创建多个很酷的特性的基础:FEN 字符串处理、ASCII 表示、PNG 图像创建、位置评估等等。

它是使用对象存储来实现的。

请记住，PHP 自带的 [SplObjectStorage](https://www.php.net/manual/en/class.splobjectstorage.php) 类允许您轻松地执行像附加、分离或计数对象这样的操作。在它的基本形式中`Chess\Board`是一个面向对象的棋子存储器。

[](/how-to-iterate-over-a-typescript-map-c62ca11cc69c) [## 如何迭代 TypeScript 映射

### 并在不使用 forEach 循环的情况下返回值

blog.devgenius.io](/how-to-iterate-over-a-typescript-map-c62ca11cc69c) 

因此，观察者模式允许在每次移动时通知棋子在`Chess\Board`发生的变化。对于棋子来说，访问棋盘对象以跟上最新的启发式评估尤其重要:空间、压力等等。

## 听起来相当不错

但是这一切如何转化为代码呢？

首先，值得一提的是，这个观察者模式的例子是使用两个 PHP 特征实现的。正如您可能知道的那样，PHP 特征是重用代码的一种很好的方式——并且在某种程度上模拟了多重继承。基本上，它们允许复制和粘贴代码，而功能在一个文件中保持独立。

棋盘和棋子是同一个硬币的两面。一方面，`[Chess\BoardObserverPieceTrait](https://github.com/chesslablab/php-chess/blob/master/src/BoardObserverPieceTrait.php)`负责将`[Chess\Board](https://github.com/chesslablab/php-chess/blob/master/src/Board.php)`中的变化通知给所有部件。

另一方面`[Chess\Piece\PieceObserverBoardTrait](https://github.com/chesslablab/php-chess/blob/master/src/Piece/PieceObserverBoardTrait.php)`基本上刷新了所有棋子中`[Chess\Board](https://github.com/chesslablab/php-chess/blob/master/src/Board.php)`的状态。

`setBoard()`方法在所有代码中都可用，因为它们扩展了使用`PieceObserverBoardTrait`的`[Chess\Piece\AbstractPiece](https://github.com/chesslablab/php-chess/blob/master/src/Piece/AbstractPiece.php)`类，如下所示。

是的，你猜对了！`Chess\Board`是主题，`Chess\Piece`名称空间中的 PHP 类是观察者。

神奇的事情发生在`Chess\Board`中的`refresh()`方法中，每次下一步棋都会调用这个方法。顾名思义，这个方法刷新棋盘的状态，相应地通知各个部分发生的变化。

有了这个软件架构，棋子可以在任何给定时刻访问底层棋盘对象以读取启发式评估(`SqEval`、`SpaceEval`、`PressureEval`和`DefenseEval`)。

都是关于通知和被通知。