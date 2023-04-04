# 在 Unity 中编写通信脚本

> 原文：<https://blog.devgenius.io/script-communication-in-unity-using-scriptable-objects-ad2ef0d99c59?source=collection_archive---------2----------------------->

## 使用脚本化对象

![](img/057383cde7e700ea59a23e2d09bd15a6.png)

可脚本化的对象使得在游戏对象之间传递数据变得容易，这对于游戏中使用的全局变量尤其有用，并且有助于保持游戏系统的模块化。例如，你有一个产卵系统，只有当玩家活着的时候才会产卵。你的用户界面系统显示分数，球员的生活和其他数据。没有玩家就不能使用产卵系统或者 UI 系统。

这是受两个 Unite talks[Unite Austin 2017——带有可脚本化对象的游戏架构](https://www.youtube.com/watch?v=raQ3iHhE_Kk)和[Unite 2016——在光荣的可脚本化对象革命中推翻 MonoBehaviour 暴政](https://www.youtube.com/watch?v=6vmRwLYWNRo)的启发。要阅读更多关于可脚本化对象的内容，请参阅 [Unity 手册](https://docs.unity3d.com/Manual/class-ScriptableObject.html)和[脚本参考](https://docs.unity3d.com/ScriptReference/ScriptableObject.html)。

我使用来自 https://github.com/roboryantron/Unite2017 的代码作为我的变量和游戏事件的基础。我为常见的变量类型 string、int、float、bool、Vector2、Vector3 创建了变量和变量引用。我从一个抽象的变量基泛型、一个接受变量基的变量引用泛型和一个静态变量引用属性抽屉(使它看起来像是内置到 Unity 中的)开始。

![](img/7f38cee41cfa3a34da5bc86664d42bef.png)![](img/058d5c5fabff2b13a800b6dbe1635394.png)![](img/0501e7986c3f23de896074469c1feb51.png)

int 类型是如何设置的，其他类型也是如此。

![](img/c90b6040101da5d6da4c48c7910bdf34.png)

注意，唯一需要的代码是 Add 方法

![](img/94e53cd11a69b5f9e3bf29883a8e06f5.png)

请注意，除了类定义之外，这里没有任何代码。

![](img/5418813166a626a54395d1949ae53c54.png)

对于属性抽屉，这是我必须做的，以使属性抽屉为每种类型工作。注意，它只是调用 Gui 方法上的变量引用属性 Drawer。

# 初始设置

![](img/252e39834d091ddb0aa979c78c3aa194.png)![](img/9a3f67f95868d62dee8fe7ff4fcd493b.png)![](img/923d6ae7b0e3b9f5f196028bc2179924.png)![](img/f0f453e6ca43b384e31b633664fa18f3.png)

为了显示玩家拥有的生命，我们需要找到玩家的游戏对象，并得到它的可损坏组件，以获得它拥有的生命数。

![](img/bd1122ded1850de35bfbf8f43fdfa257.png)![](img/04034dbc5714b8b7e5ca49d34be8104b.png)![](img/a30a220249353e9f43fae0a4201b1a9d.png)![](img/74009c450be43da927940cb2d671a637.png)

# 使用可编写脚本的对象初始化变量

UI 系统不关心它显示的值来自哪里，只要它有它需要的信息。让事情变得更简单，我们的游戏性能和使用一个可脚本化的对象 Int 变量。

## 更新生命更新程序脚本

我将在这里使用 Int 引用类型，以防游戏中没有这个变量，我们只是使用 UI，我们可以在检查器中调整这个值来测试我们的 UI 是否工作。

![](img/e96b1ca97a07ad6085f076cd669b8ed1.png)

更容易得到我们想要的信息。

![](img/74009c450be43da927940cb2d671a637.png)

## 更新易损坏的脚本

我们在这里使用 Int 引用类型，所以我们不必为游戏中的每个游戏对象都设置一个可脚本化的对象变量。玩家的生命是我们唯一需要的。我们将允许游戏设计师决定他们需要哪些变量。

![](img/6da1f381eae027810945f65cb2efeca6.png)![](img/e1b09e196ddb22732f530072878b4960.png)

## 使用可编写脚本的对象变量

创建变量。

![](img/057383cde7e700ea59a23e2d09bd15a6.png)

在游戏中使用变量。

![](img/7e1d60423cfcdcda28d6502da8a13bde.png)

属性抽屉的目的是在检查器窗口中绘制漂亮的界面。您确切地知道组件使用什么作为它的数据。如果没有属性抽屉脚本，常量值和变量值都将显示在检查器中，这可能会引起混淆。

使用脚本化对象 Int 变量的游戏

![](img/6347ec20a41c57e5b135f481cae39a53.png)

# 对运动方向使用矢量 3 可脚本化的对象变量

更新播放器和可移动代码以使用力矩方向。

![](img/9b15b6731453c287ea778e3765eb7f2a.png)

玩家现在使用向量 3 变量

请注意，我使用的是变量，而不是变量引用。我只是展示了在代码中使用两种不同类型的区别。

对于变量类型，您必须使用 SetValue 或 Add 方法来更改它。

![](img/92a67a2560b6e03b8ed2ab6bb8d88113.png)

现在可使用 Vector3 参考移动

创建玩家移动方向变量，并设置移动方向来使用这个变量。因为我们改变了类型，所以我们必须去游戏中的每个游戏对象，并确保移动方向有正确的值。

![](img/d78c3526bdc4ae544fde24bc176fba18.png)

# 游戏也一样。

请注意，当我移动并受到伤害时，你可以看到数值实时更新。这些值会在播放模式中更改，离开播放模式后，它们不会像在播放模式中更改的其他变量一样重置。

![](img/c8283bb14e28e94fa9d6547f4f38d7e7.png)