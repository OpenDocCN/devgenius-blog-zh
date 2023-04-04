# 在 Unity 中创建一个战利品系统

> 原文：<https://blog.devgenius.io/create-a-loot-system-in-unity-2576f7eb10d5?source=collection_archive---------2----------------------->

## 使用脚本对象和预设

![](img/a84aa8b3a512be975b7eb72c5c227436.png)

我创建了一个 Gem ScriptableObject 来跟踪我的玩家有多少宝石。关于 ScriptableObject 变量的更多信息，请参见 GIT hub 上我的 [**核心框架**](https://github.com/JamesLaFritz/CoreFrameWork) 。

![](img/02b376ddacea30850c8a04130aa3a374.png)

我为我的敌人创造了一个预置，一旦他们被杀死，我就可以把它丢进游戏。

![](img/7a49daac562fb6605386d2e97962e274.png)

菱形有一个行为，当玩家与它碰撞时，它会将菱形的值添加到 ScriptableObject 中。然后它会自我毁灭。

![](img/5393bae60860a82baa29aa9b24fa33ac.png)

我确保我的钻石和钻石掉落预设使用我之前创建的这个 ScriptableObject。两个预设之间的区别是钻石掉落预设有一个额外的碰撞器来防止它从地面掉落，并使用重力。

![](img/ba4c6c4f1888b752f887ca11513755d3.png)

我的敌人有一个生命值属性，当它被设置时，如果它没有死，它会设置敌人的生命值。如果敌人的生命值小于 1，那么它会设置死亡属性为真。

![](img/a39329f829b0d43b71196fc53a998e1c.png)

当敌人死亡时，我有几种不同的方法可以给玩家宝石或战利品。因为我的游戏中唯一的战利品是钻石，所以我将为敌人的每个宝石生成一个钻石预置(如果有的话),为此我使用了一个 for 循环，我可以引用生成的游戏对象，并将值改为宝石的数量。然后我在设定的时间后消灭敌人，这样死亡动画就有足够的时间播放了。

![](img/bc5f09fe9ef163b726491470036bcb4c.png)

我编辑我所有的敌人预设，用新的参数更新它们。

![](img/c45f994e663e917e04e90149bdb64838.png)

现在我可以拿回我的战利品了。不管它是被其中一个敌人弄掉的还是来自其他地方的。这些系统是完全独立和模块化的。

![](img/7fe88a9feb5e9b2d0ea04be66dc0c5b3.png)![](img/faf11237bb22a70d895e57515a5bd2aa.png)![](img/6893bc4647fb7aac162d8438984461c0.png)