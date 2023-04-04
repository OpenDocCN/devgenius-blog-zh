# 项目中的设计模式——模板和策略

> 原文：<https://blog.devgenius.io/design-patterns-in-project-template-strategy-a3a4c32e64dc?source=collection_archive---------11----------------------->

![](img/6ff14ce16dbf1c65d92e3544c68eb34b.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

一个**设计模式**为软件设计中出现的常见问题提供了一个通用的可重用解决方案。这个想法是通过类或对象之间更好的关系和交互来加速开发过程。结果是更加灵活、可重用和可维护的代码。

**模板**和**策略**就是两个这样的模式，可以帮助我们构建代码。我在网上看到过很多例子，但我觉得通过项目实例来解释一下它的用法会很好。

**让我们考虑一个场景**和可能的解决方案。

在我之前的博客中，我解释过关于[吃角子老虎机](https://medium.com/@deepakjoshi14/designing-a-casino-slot-game-math-2d35d25f0a74)和[不同玩家参与](https://medium.com/@deepakjoshi14/understanding-different-players-involved-in-casino-game-development-e67ad4a7bae)。在吃角子老虎机游戏中，我们有一个网格，赢的计算基于网格中可用的符号。然后，获胜以可展示的形式显示给玩家。

**服务器端场景:**
在一个网格中，有很多方法可以计算 wins。

*   预定中奖组合中的符号。
*   连续卷轴上的相同符号。
*   符号群。

可能会有触发功能的场景。

*   网格触发功能中的 3 个或更多同类符号
*   卷轴触发特征中的相同符号
*   随机特性被触发

**客户端场景:**

不同的客户对游戏有不同的要求。他们希望他们的游戏遵循一个特定的模式，这样玩家就能识别这个游戏。让我们考虑两个功能。

符号动画

*   所有获胜的组合应该只制作一次动画
*   所有获胜的组合应该保持循环动画
*   获胜组合应该一个接一个地显示出来

玩家赢款显示

*   勾选赢得金额，在 x 秒内完成
*   根据特定算法计算赢款

对于上面提到的两种情况，有三种可能的解决方案。

1.  **我们使用最常用的场景创建默认代码。如果任何游戏需要，我们会覆盖游戏中的代码。在这种情况下，我们会遇到这样的问题，代码重复，我们在每个游戏中都有未经测试的代码，没有可重用性，更多的开发工作，垃圾代码，等等。**
2.  我们可以为每个客户创建不同的库。在这种情况下，我们会遇到维护问题。很快我们将看到不同的开发者开发不同版本的库，每个游戏都有不同的代码来实现相同的功能。对于游戏的维护来说会是一个很大的风险。
3.  **用户设计模式更好地设计图书馆**。

**服务器端场景解决方案**

让我们创建接口*wine evaluator*和 *FeatureEvaluator* 。

```
interface WinEvaluator { 
 function evaluate();
}interface featureEvaluator { 
 function evaluate();
}
```

接下来，创建一个接受实现这些接口的类并调用它们的函数的类。

```
*class Evaluator {*  *private WinEvaluator win;
    private FeatureEvaluator feature;* *public Evaluator(* WinEvaluator win, *FeatureEvaluator feature){
        this.win = win
        this.feature = feature
    }* *public evaluate(){
        win.evaluate();
        feature.evaluate();
    }**}*
```

现在，我们只需要使用接口创建不同的类。

```
class PredeterminedWinCombination implements WinEvaluator {
    public evaluate(){
    }
}class SymbolInConcecutiveReel implements WinEvaluator {
    public evaluate(){
    }
}class ClusterOfSymbols implements WinEvaluator {
    public evaluate(){
    }
}class SymbolOfAKind implements *FeatureEvaluator* {
    public evaluate(){
    }
}class StackedSymbol implements *FeatureEvaluator* {
    public evaluate(){
    }
}class RandomFeature implements *FeatureEvaluator* {
    public evaluate(){
    }
}
```

我们结束了。现在我们只需要在游戏中调用需要的类。

```
Evaluator eval = new Evaluator( 
              new ClusterOfSymbols(), new StackedSymbol() 
);
eval.evaluate();
```

**解释代码**

我们所做的是创建一个模板*“Evaluator”。*这个类知道所有的函数被调用的内容以及调用的顺序。

接下来，我们创建了接口。我们的函数类扩展了接口。这些类根据需求从外部注入到我们的主类" *Evaluator* "中。所以我们可以在任何排列中使用它们。

结果，

*   类，经过测试，我们有信心。我们可以对类进行单元测试。
*   代码可重用性。没有重复的代码。没有垃圾代码。
*   每个人都使用同一个库，只在需要的时候调用类。
*   易于维护。要改变功能，只需改变类对象。

**客户端场景解决方案**

让我们创建界面*符号动画*和 *WinAmountTickup* 。

```
interface SymbolAnimation { 
 function animateSymbols();
}interface WinDisplayTickup { 
 function tickupWin();
}
```

接下来，创建一个接受实现这些接口的类并调用它们的函数的类。

```
*class ShowWinPresentation{* *private* WinDisplayTickup *tickup;
    private* SymbolAnimation *animation;* *public ShowWinPresentation(* WinDisplayTickup *tickup*, SymbolAnimation *animation){
        this.tickup = tickup
        this.animation = animation
   }* *public show(){
        tickup.*tickupWin*();
        animation.*animateSymbols*();
   }**}*
```

现在，我们只需要使用接口创建不同的类。

```
class AllSymbolsAnimateOnlyOnce implements SymbolAnimation {
    public animateSymbols(){
    }
}class AllSymbolsAnimateInLoop implements SymbolAnimation {
    public animateSymbols(){
    }
}class SymbolsAnimateOneByOne implements SymbolAnimation {
    public animateSymbols(){
    }
}class FinishInXSeconds implements WinDisplayTickup{
    public tickupWin(){
    }
}class FinishUsingABCAlgo implements WinDisplayTickup {
    public tickupWin(){
    }
}class FinishUsingXYZAlgo implements WinDisplayTickup {
    public tickupWin(){
    }
}
```

我们结束了。现在我们只需要在游戏中调用需要的类。

```
*ShowWinPresentation* winPresentation = new *ShowWinPresentation*( 
         new AllSymbolsAnimateInLoop(), new FinishUsingABCAlgo() 
);
winPresentation.*show*();
```

您可以看到，即使在这里，我们也使用了类似的模式。有一个带有预定义功能流顺序的模板类(可以按照我们想要的方式控制)，我们在外部为它提供了每个功能的类。

这样，我们可以轻松地管理我们的代码，并为团队提供更好的开发方法。

我希望我的文章对你理解如何使用策略和模板设计模式有所帮助。

谢谢你