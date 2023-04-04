# 如何修复 Unity3d 中的游戏管理器问题

> 原文：<https://blog.devgenius.io/how-to-fix-your-gameplay-manager-problems-in-unity3d-d412cc2b12d0?source=collection_archive---------23----------------------->

![](img/1c392e49b663058929fb377602008a7e.png)

游戏程序员最常见的问题之一是创建易于访问的管理器，而不使用单例模式和其他模式，这些模式会产生比他们解决的问题更多的问题。多年来，我一直在研究多个引擎的代码，以了解它们如何实现这一点，并创建了一些自己的实现，现在我想向您介绍一个 Unity 中游戏管理器架构的解决方案，该解决方案受 UE4 游戏系统架构和 DOTS 实现中新的管理器 Unity 架构的启发。

让我们从为游戏管理器创建一个基类开始:

更新方法在那里，所以你可以创建经理不是单一的行为，而是在一个游戏对象的工作流程中调用。还可以重写 OnCreateManager 和 OnDestroyManager 方法，以便可以分别进行初始化和清理。

现在来看看这个解决方案的真正内容，游戏实例类:

它维护了一个字典和一个管理器列表，以使访问和迭代都具有最佳性能。

要获得特定类“ManagerClass”的管理器，可以这样使用它:

var manager = RstGameInstance。GetOrCreateManager<managerclass>()；</managerclass>

就是这样。如果你喜欢这个故事:

就像 fb 上的摇滚广场惊雷:
【https://www.facebook.com/rocksquarethunder】T2

在推特或 Instagram 上关注我们:【https://twitter.com/thunder_square】
[https://www.instagram.com/rocksquarethunder/](https://www.instagram.com/rocksquarethunder/)

加入我们的纷争:
[https://discord.gg/bukwCw7](https://discord.gg/bukwCw7)