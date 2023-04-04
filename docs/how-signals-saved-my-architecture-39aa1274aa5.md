# 信号如何拯救了我的建筑

> 原文：<https://blog.devgenius.io/how-signals-saved-my-architecture-39aa1274aa5?source=collection_archive---------16----------------------->

![](img/beb16c50b78d5e4a2807c3aac1569624.png)

程序员桌

我现在做游戏 7 年，编程 10 年，还不算我学计算机的那几年。我写过好的代码，坏的代码和难看的代码。我在各种规模的 AAA，AA 和独立工作室工作过。UE4、Unity 和内部引擎。我的观点是:我在 gamedev 内外见过很多不同的项目架构。

当谈到 C#和 Unity 时，我一直在努力寻找以如下方式编写代码的方法:

-可扩展

-高性能

-易于调试

-可维护

-易于教授团队中的新程序员

我尝试过使用像 Zenject 这样的依赖注入框架，但是它混淆了调用栈，使得调试变得更加困难，尽管我一直在尝试教授 SOLID etc。对于团队的所有新成员来说，通常他们使用它的方式实际上比根本不使用任何框架更糟糕。尤其是单接口原理是很难维护的。

然后我找到了一个非常小的库，叫做 Signals:[https://github.com/yankooliveira/signals](https://github.com/yankooliveira/signals)

这个想法是众所周知的——事件驱动的通信，但它的编写方式使它非常容易使用，它使用类型作为事件的 id，所以你不需要在发送者和接收者之间创建依赖关系。

**例证时间。**

假设你在做一个平台游戏。每当你跳跃的时候，你可能想做一些事情，比如改变动画状态，产生粒子，播放声音等等。这是你用信号做的:

1.  **创建一个 JumpSignal 类，如下所示:** *公共类 Jump signal:ASignal { }*这是其他系统调度和响应跳转事件时唯一需要知道的事情
2.  **在你的跳跃能力中，你实际上执行了一次跳跃，你发出了这样的事件:** *信号。获取<跳转信号>()。分派()；*你不需要关心谁对这个事件做出响应，以及如何响应
3.  **在你的动画师/粒子/音频管理器中你这样订阅这个事件:** private void one nable()
    {
    信号。获取<跳转信号>()。add listener(on jumped)；
    }
    私有 void OnDisable()
    {
    信号。获取<跳跃信号>()。remove listener(on jumped)；
    }
    void on jumped()
    {
    //下面是您响应事件
    的代码
4.  您不需要知道这个事件来自哪里，也不需要依赖跳转实现。
    你可以用其他代码替换你的跳转代码，只要它调度了事件，你就设置好了。
5.  **就这样。它是如此的简单和干净。**

所以继续试着发出信号。这太神奇了，它会让你的代码变得更好。如果你想和我讨论任何与游戏开发或代码相关的事情，你可以:

喜欢 fb 上的摇滚广场雷霆:[https://www.facebook.com/rocksquarethunder](https://www.facebook.com/rocksquarethunder) 在 Twitter 上关注我们:
@雷霆 _ 广场
或者加入我们的不和谐:
[https://discord.gg/bukwCw7](https://discord.gg/bukwCw7)