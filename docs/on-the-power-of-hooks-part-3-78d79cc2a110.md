# 钩子的力量:第三部分

> 原文：<https://blog.devgenius.io/on-the-power-of-hooks-part-3-78d79cc2a110?source=collection_archive---------1----------------------->

![](img/ad64972cc1e1ba7ec19e61f251ce4f7d.png)

由 [Vishal Jadhav](https://unsplash.com/@vishu_2star?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我们来到了 React Hooks 系列文章的最后一部分。在第 1 部分中，我们已经学习了如何使用钩子让我们的组件播放音频文件。在[第 2 部分](https://medium.com/@accounts_32070/on-the-power-of-hooks-part-2-8de86630648c)中，我们已经学习了如何增强一个组件，使其始终知道当前时间。现在是时候将所有这些拼凑在一起，并探索我们如何在现实生活中利用我们所学到的东西。

# 上船啦

因此，让我们想象一下，一家公司希望鼓励其员工在轮班时定期洗手。高管们已经决定推出一款非常简单的应用程序，该程序会提醒员工每隔一段时间洗一次手。我们现在被指派开发这样的应用程序，在开发的第一天，这些用户故事就摆在了我们的桌子上:

*   作为一个用户，我想在我轮班开始时签到。
*   作为一名用户，我希望看到一个提醒，提醒我当前正在值班
*   作为一名用户，我想手动下班

让我们看看如何利用钩子来完成这项工作。

# 周一忧郁…

我们想做的第一件事就是为我们的轮班数据建立一个模型。我们将利用 [redux-toolkit](https://redux-toolkit.js.org/) 创建所有必要的代码来存储和操作我们的用户转换状态。

一个简单的切片，可以在轮班时检查和离开。

如果你不熟悉[切片](https://redux-toolkit.js.org/api/createSlice)，它们可以非常方便地声明状态，而且是从一个地方的突变。上面的代码让我们分别通过`shiftSlice.reducer`和`shiftSlice.action`来访问一个缩减器及其动作。

让我们将它应用到第一个组件中，一个类似按钮的视图，它允许用户在相应地改变颜色的同时进出班次。

良好的第一步。

我们来分解一下。

我们利用前一篇文章中的`useCurrentTime`来引用当前每秒更新的内容。然后我们通过 redux 的`useSelector`使用`lastShiftSelector`来获得最新的内存转移。

这个有趣的位在`useEffect`中，每次`currentTime`改变时都会被调用，然后检查`currentTime`是否在最后一个班次的`endTime`之前。如果是，我们在轮班中，状态通过`setIsOnShift`相应地更新。

然后，我们的`TouchableOpacity`负责根据用户输入更新换档状态。检查我们是否正在值班，或者检查我们是否外出。这是通过由`useDispatch`分派的片的动作来完成的。

到目前为止，我们已经处理了故事 1 和 3，现在我们希望有一个单独的文本提醒来显示用户是否在轮班。可以想象，我们必须将上面的大部分代码复制到另一个组件中，以检查用户是否在值班。那么我们为什么不利用我们所学的知识，从上面的组件中提取一个自定义钩子呢？让我们试试。

不错！

好了，我们已经抽象出了`useIsOnShift`，现在我们可以在我们的代码库中使用我们的定制钩子了。我们现在可以重构`ShiftButton`来使用我们的钩子。

我们还设法节省了整整 10 行代码。

然后，我们可以利用`useIsOnShift`来完成我们的第二个任务。

我们的徽章现在只有在用户轮班时才会出现。

我们现在已经完成了分配给我们的所有用户情景。

**去庆祝！**

这是我穿越 React Hooks 之旅的第三部分。你可以在这里找到[第一部分](https://medium.com/@accounts_32070/on-the-power-of-react-hooks-49094e76709c)和在那里找到[第二部分](https://medium.com/@accounts_32070/on-the-power-of-hooks-part-2-8de86630648c)。