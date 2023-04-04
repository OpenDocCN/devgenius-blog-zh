# Unity 新输入系统第 1 部分

> 原文：<https://blog.devgenius.io/unity-new-input-system-part-1-d458ba61e8cd?source=collection_archive---------1----------------------->

## 使用新的输入系统、玩家输入和 Unity 事件。

![](img/9be7dd389b449c54f3485435618ccd85.png)

Unity 曾经有一个跨平台工具包，我可以用它来做输入。但可悲的是，它已经贬值。

[https://forum . unity . com/threads/crossplatforminput-deprecated . 539339/](https://forum.unity.com/threads/crossplatforminput-deprecated.539339/)

Unity 有一个新的输入系统，这是他们推荐的输入系统。

[](https://resources.unity.com/unitenow/onlinesessions/input-system-workflow-tips-and-feature-integrations) [## 输入系统:工作流程提示和功能集成

### 来自我们技术营销团队的 Andy Touch 向您展示了在开发跨平台时如何解决常见的情况…

resources.unity.com](https://resources.unity.com/unitenow/onlinesessions/input-system-workflow-tips-and-feature-integrations) 

看一下支持的设备，集成我想要的输入似乎很容易。

 [## 支持的输入设备

### 本页列出了输入系统包支持的输入设备类型和产品，以及它们支持的平台…

docs.unity3d.com](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/SupportedDevices.html) 

# 安装

为了安装它，我使用了包管理器。

![](img/17973078175aa79a6fa02f2d9c757478.png)

它问我是否要启用后端，我说是，然后它重启了 Unity。

如果您需要帮助，请参阅本指南。

[](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/Installation.html) [## 安装指南

### 本指南介绍了如何为您的 Unity 项目安装和激活输入系统包。注意:新输入…

docs.unity3d.com](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/Installation.html) 

# 间接输入和玩家输入行为

为了支持多种类型的输入，而不必做任何额外的编码，我将通过一个输入动作间接获得输入。

[](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/QuickStartGuide.html#getting-input-indirectly-through-an-input-action) [## 快速入门指南

### 注:有关如何安装新输入系统的信息，请参见安装。开始使用…的最快方法

docs.unity3d.com](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/QuickStartGuide.html#getting-input-indirectly-through-an-input-action) 

新的输入系统带有玩家输入行为。我将使用它为我的播放器获取输入。我在我的播放器上添加了这个组件。我还使用 Create Actions 来创建 Actions 资产，其中已经设置了一些默认值。

![](img/0f3e5fddef0ad6673ed846a48c02a8d2.png)

我将新创建的动作资产分配给玩家输入，并查看定义的动作。它必须有动作映射，一个用于 UI，一个用于播放器。看起来动作是 UI 的标准。玩家有 3 个动作，一个眼神和一个火焰。“移动”和“看”是具有向量 2 的价值动作类型。火是按钮式的。

![](img/717fd47b61ddfc63615fd62f2cbf797e.png)

## 行动资产

移动使用操纵杆、游戏手柄上的左摇杆、XR 控制器和标准的 WASD 箭头键来移动。外观是为所有设备上的标准相机控件设置的。火是为标准的射手控件设置的。我将只使用移动动作。

![](img/351b0b4a946f4266abbe3fe19a528eb8.png)

## 关于玩家输入的更多信息

玩家输入带有很多我可以设置的选项。我正在为 Android 设计这个游戏，但是我想我会保留默认的模式，保持自动切换。我只能从玩家和用户界面中选择 2 张地图，我知道我想使用玩家地图。我现在不关心用户界面，我在游戏中没有设置用户界面菜单，我会把它设置为无。我没有使用输入在场景中移动我的相机，所以我将把它留空。对于这种行为，我现在要用统一事件。在这四个选项中，统一事件和升 C 调事件将是性能最好的。如果你想使用“发送消息”见 [**脚本 API 为玩家输入**](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/api/UnityEngine.InputSystem.PlayerInput.html) 注:使用`Update not OnUpdate`

![](img/1beec51929c8bf850392307d5d550a80.png)

因为这是玩家，这是移动事件，我要让它移动玩家。有了 Unity Events，我可以在任何行为的任何组件上使用任何带 0-1 参数的公共方法。为了充分利用正在使用的输入，我创建的方法需要一个 [**输入操作。回调上下文**](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/api/UnityEngine.InputSystem.InputAction.CallbackContext.html) 参数。

![](img/f5328294dc1b6735303dd490c4647d8d.png)

设置播放器输入以使用我创建的事件。

![](img/19da04116e69d91b35d256b6f8201b52.png)

测试这个。

![](img/960475cd39643e2f0a69e9efa21cf992.png)

## 使用交互

 [## 相互作用

### 交互代表一种特定的输入模式。例如，暂停是一种需要控制的交互…

docs.unity3d.com](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.0/manual/Interactions.html) 

有几种交互可以添加到一个动作中来增加控制。

![](img/c84042486941273cdc3159744b8c58f4.png)

我的控制台中有几条不同消息的原因是，事件在不同的阶段启动、执行、取消时被触发

![](img/a15808cc5ba995362d944bb74a624ef6.png)

为了在我的代码中完全实现这一点，我需要使用动态回调

![](img/728aa031e76d44be1b31970a0d331651.png)

要执行这个动作，我需要在我设定的时间内按住发射按钮，要再次执行这个动作，我必须松开发射按钮并再次按下它。

![](img/770d5bd54b50ab4aed85d8c8766c5df6.png)

我可以用互动来做一个冲锋效果，比如塞尔达旋转攻击，银河战士冲锋效果。在动作阶段，只要按钮被按下，我就会自动开火。我可以执行的行动，如果按钮是按下这一帧，或释放这一帧。我可以在 On Fire 事件中使用它，或者缓存输入操作所需的值，并在更新方法中使用它们。

![](img/24b217c7e823e2c71728ee27b2aac2e8.png)