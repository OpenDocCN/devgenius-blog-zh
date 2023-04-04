# 有效的 Windows 窗体开发技巧

> 原文：<https://blog.devgenius.io/effective-windows-forms-development-tips-ed0a27d887c2?source=collection_archive---------14----------------------->

## 对于图形和多线程

![](img/b6e3897cfe1f8715e01937f30928c49e.png)

[https://www.pexels.com/photo/mosaic-alien-on-wall-1670977/](https://www.pexels.com/photo/mosaic-alien-on-wall-1670977/)

Windows Forms【WF】卷土重来。NET Core 显著的性能提升，在本文中，我们将介绍一些在开发 WF 应用程序时需要牢记的技巧和技术。首先，我们将介绍实例检测，然后使用内置的*背景工作器*，最后我们将学习如何在我们的 WF 中绘制、无效和模拟一个游戏循环，而没有烦人的闪烁。

过去，许多开发人员更喜欢与 WF 合作的原因之一是，他们可以轻而易举地原型化和构建一套良好的功能。这是通过可重写事件、控件和编辑器的组合来实现的。

让我们从一个小小的误称开始:检测一个应用程序运行的多个实例，而不是检测单个应用程序中打开的一个表单的多个实例。后者可以通过查询*应用来实现。例如，当试图判断应用程序实例是否已经在运行时，一个简单的解决方案是查询当前进程并与活动进程进行比较。*

R 使用内置[后台工作器](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.backgroundworker?view=net-5.0)运行长时间操作。该类提供了一个用于执行长时间运行操作的专用线程和一组事件，以便对操作的不同阶段进行粒度控制。下面几行说明了如何设置一个 [BackgroundWorker](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.backgroundworker?view=net-5.0) 的实例。

W 当使用 *BackgroundWorker* 或多线程时，对于某些控件更新，正在运行的线程很可能不会与 UI 线程同步(RunWorkerCompleted 确实会同步，但 DoWork 内的 UI 更新会导致 InvokeRequired 标志被设置为 true)。框架处理这种情况的方式是在从控件基类派生的任何东西上翻转 InvokeRequired 标志。有了这些知识，我们可以编写一个简单的扩展来简化 UI 更新的执行，而不必担心线程的同步。

任何 UI 控件都可以使用下面的语法调用这个扩展。thread safe update(()=>textbox 1。Text =日期时间。utc now . ToLongTimeString())；

W F 并不特别适合制作视频游戏，因为绘制操作使用较慢的 GDI+API，并且框架提供的开箱即用定时器不是高分辨率定时器。借鉴 WF 仍然是一个有趣的活动，有一些技巧(很容易快速完成)，甚至创造一些事件驱动的游戏，如扫雷或棋盘游戏，如国际象棋。在定时器上渲染或调用 Invalidate()进行即时重绘时，需要注意的一点是，必须使用 VSync 或帧速率与显示器刷新率同步，以避免闪烁或屏幕撕裂。将 DoubleBuffered 属性设置为 true 可以避免闪烁。

随着我们的闪烁问题的解决，让我们使用系统创建一个小的游戏循环。计时器。计时器代替了表格计时器，以获得更好的准确性和控制。

这里需要注意的一点是，在整个窗体区域的不同位置将绘制一个矩形，但是为了维护所有以前的矩形，需要将这些矩形添加到一个集合中，并在一个循环中绘制。