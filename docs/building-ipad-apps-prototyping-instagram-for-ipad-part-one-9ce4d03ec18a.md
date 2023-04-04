# 构建 iPad 应用程序，为 iPad 开发 Instagram 原型。

> 原文：<https://blog.devgenius.io/building-ipad-apps-prototyping-instagram-for-ipad-part-one-9ce4d03ec18a?source=collection_archive---------1----------------------->

![](img/f9c667d5793359839a6ccb222c4b755a.png)

Instagram iPad 原型

在本帖中，我们将谈论 iPad 的制作。我们将使用 **Instagram** 的原型来强调操作系统如何管理其布局系统以适应不同的 iPad 设备和多任务环境的关键主题。不知道为什么 **Instagram** 还不支持 iPad，但我确信它可能很快就会推出。原型看起来是这样的…

![](img/b6eb581c6a3e45dbae3789f5c93dedd0.png)

Ipad Instagram。

*它根据设备方向调整布局。*

![](img/b6d09f33144242a786abad1ecc4edd59.png)

更改横向和纵向模式的布局。

*它根据尺寸等级调整用户界面。*

![](img/56082d6aba7d250052c29e552a64b320.png)

在紧凑/常规 traitcollection 环境中调整文章标题的大小。

*适应 UI 监听 iPad 显示模式变化。*

![](img/da44d1a4637497dd48fe9f0d32fc3d53.png)

基于拆分视图控制器显示模式隐藏/显示突出显示用户界面。

你可以在这里找到完整的报告，它包含模拟图像和模型来显示用户界面，还混合了一些技术，包括泛型和使用最新的 iOS 13 API，如不同的数据源和组合布局，但是在这篇文章中，我们不会讨论如何构建模型、视图或布局，(也许我会再写一篇文章来讨论如何使用组合布局来构建用户界面)。🙃

相反，我们将关注于`UISplitViewController`的细节以及它如何为不同大小的类工作。

如果你想了解更多关于泛型和`UICollectionViewDiffableDataSource`的知识，你可以点击[这里](https://medium.com/@jamesrochabrun/advance-generics-to-create-reusable-ui-f0b8b8934895)和[这里](https://medium.com/@jamesrochabrun/uicollectionviewdiffabledatasource-and-decodable-step-by-step-6b727dd2485)。🤓

如果这听起来很有趣，这里是我们将涵盖的主题列表，如果你只是想加入代码或甚至做出贡献，这里是[回购](https://github.com/jamesrochabrun/InstagramIpad)。

# 第一部分

*   UISplitViewController 整体。
*   UISplitViewController 显示模式。
*   面向协议的用户界面更新。
*   UISplitViewController 处于折叠状态。
*   UISplitViewControllerDelegate。

# 什么是`UISplitViewController?`

来自苹果公司:

> *拆分视图控制器是一个容器视图控制器，它在一个主从界面中管理两个子视图控制器。在这种类型的界面中，主视图控制器(主视图)中的更改会驱动次视图控制器(细节视图)中的更改。两个视图控制器可以并排排列，这样一次只能看到一个，或者一个只能部分隐藏另一个。在 iOS 8 及更高版本中，可以在所有 iOS 设备上使用* `*UISplitViewController*` *类；在 iOS 的早期版本中，该课程仅在 iPad 上可用。*

这意味着什么呢？它基本上管理着一个导航层次结构，其中包含主要或“主”(左视图控制器)和次要或“细节”(右视图控制器)关系，它们也可以有自己独立的导航层次结构。它的外观由您安装的子视图控制器定义，设置它的`viewControllers`属性。

它还可以响应`traitCollection`的变化，在允许推送导航的水平紧凑环境中，将主视图和详细视图“合并”在一个单独的导航堆栈中，这就是它甚至可以在 iPhone 设备中工作的原因📱。

# Instagram iPad 原型。

好的，假设我们想要遵循 **Instagram** 的设计，让我们的根视图控制器成为一个标签栏控制器，就像这样(在我们的演示中，我们使用的是 12.9 英寸的 iPad 第三代 iOS 13.3)…

![](img/d20a41bd154ca728912a44597cc1f64d.png)

使用 UISPlitViewController 的配置文件选项卡。

在这里，您可以看到 profile 选项卡是一个主-详细关系，它在左侧显示用户的内容(主要)和一个空的详细信息(次要)。一旦用户执行选择，细节将会改变。*(这是一种常见的 iPad 设计，在启动时细节是一个空白的占位符，直到用户从主视图控制器执行选择)。*

那么当你实例化一个`UISplitViewController`时，代码看起来是什么样的呢？我们有一个名为`SplitViewController`的子类，在这个代码片段中，你可以看到如何实例化它，并将其作为标签栏项目嵌入到标签栏控制器中。

开始设置代码。

1.  我们有一个叫做`SplitViewController`的子类，它有一个方便的初始化器，接受一个`viewControllers.`数组
2.  `UISplitViewController`有一个`viewControllers`属性，它接受一个 vc 的数组，不知道为什么它接受一个数组，如果只使用数组中的第一个和第二个元素来显示主元素(索引 0)和细节元素(索引 1)并丢弃其余的元素。🤷‍♂️
3.  `preferredDisplayMode,`一个控制主视图控制器(主视图)如何隐藏和/或显示的动画属性，我们将在后面详细讨论，现在将其设置为`.allVisible`将一起显示主视图和次视图。
4.  正如您所看到的，`SplitViewController`使用了`ProfileViewController`和一个`EmptyDetailViewController`(任何一种您想在启动时显示为细节的视图控制器)的实例，您甚至可以**理想地**显示由主视图中的第一个可选元素填充的细节。稍后，它只是给`tabBarItem`图像分配一个图标。
5.  `TabBarController`子类将`viewControllers`设置在`viewDidLoad`上，因为`profileViewController`是数组中的元素之一。(我们有一个 MVVM 设置，它在 [repo](https://github.com/jamesrochabrun/InstagramIpad/blob/master/SplitViewControllerTutorial/ViewControllers/Roots/TabBarController.swift#L24) 中实例化应用程序的每个选项卡)。

# UISplitViewController 显示模式

正如你在上面看到的，在第 3 步，我们可以通过设置`prefferedDisplayMode.`来设置分割视图控制器排列

> *控制主视图控制器如何隐藏和显示的动画属性。值“uisplitviewcontrollerdisplaymodeautomatic”指定默认的拆分视图控制器行为，在 iPad 上，它对应于纵向的叠加模式和横向的并排模式。*

这个属性是一个`UISPlitViewController.DisplayMode`的枚举实例，这里是一个应用程序如何寻找每种情况的例子。

## 案例自动:

> *分割视图控制器根据设备和当前应用程序大小自动决定最合适的显示模式。您可以将该常量指定为* `[*preferredDisplayMode*](https://developer.apple.com/documentation/uikit/uisplitviewcontroller/1623170-preferreddisplaymode)` *属性的值，但是* `[*displayMode*](https://developer.apple.com/documentation/uikit/uisplitviewcontroller/1623194-displaymode)` *属性不会报告该值。*

在常规的水平宽度环境中，它会像这样显示应用程序...

![](img/ceeb1b2db015e40c907409b36fc5af47.png)

主要隐藏在水平常规宽度环境中。自动的

## 案例主要隐藏:

> *主视图控制器被隐藏。*

![](img/6568439e2da56b4bd7ef850c69ea2b21.png)

初级隐藏。

## 凯斯。主要覆盖:

> *主视图控制器位于次视图控制器之上，使次视图控制器部分可见。*

![](img/d339d9317509b2916bd7db68f7c7cbf4.png)

。初级覆盖

## 凯斯。全部可见

> *主视图控制器和次视图控制器并排显示在屏幕上。*

![](img/7d12ece2dee17784a1dd19d711a6bb93.png)

主次分明。

在我们的原型中，我们在启动时使用`.allVisile` 状态，但假设我们想扩展细节视图控制器以全屏显示帖子，在一天结束时 **Instagram** 就是关于内容看起来有多好，对吗？类似这样的…

![](img/e03a31cd83c198f3f103fad9749aa6fb.png)

展开“细节”隐藏主视图控制器。

这很容易实现，而且`UISplitViewController`和它的代理免费给了我们这个！你只需要在细节视图控制器中添加一个`displayModeButtonItem`。

> *系统栏按钮项，其操作将根据 targetDisplayModeForActionInSplitViewController 的结果更改 displayMode 属性:。当插入到辅助视图控制器的导航栏中时，它将改变其外观以匹配其目标显示模式。当目标显示模式为 PrimaryHidden 时，它将显示为全屏按钮，对于 AllVisible 或 PrimaryOverlay，它将显示为后退按钮，当它不会导致任何操作时，它将变为隐藏。*

正如你在 gif 中看到的，在选择一篇文章之后，空的视图控制器占位符变成了显示当前选择的不同的细节视图控制器，我们将在第二部分中更多地讨论导航，但是现在让我们把焦点放在位于`ContentDetailViewController.`左上角的显示模式按钮上

点击它隐藏主视图控制器，并使细节全幅，这两行在视图控制器中你想要显示按钮的地方就是你所需要的。

```
/// 1
navigationItem.leftItemsSupplementBackButton = **true**/// 2
navigationItem.leftBarButtonItem = splitViewController?.displayModeButtonItem
```

1.  `leftItemsSupplementBackButton`可以在水平紧凑宽度环境中显示后退按钮，而不是展开箭头。
2.  我们只需要将`displayModeButtonItem`分配给`viewController`左栏按钮项。

这很酷，但我对它的外观并不信服，当显示模式为`.primaryHidden`时，我不想要一个后退按钮，这是 API 默认给我们的；我想自定义按钮，以添加一个更合适的图标。此外，细节视图看起来边对边都很大，一个好的填充会让它看起来更好，而且只是为了好玩，让我们利用空间的大小，让我们在顶部显示高光旋转木马，就像这样…

![](img/da44d1a4637497dd48fe9f0d32fc3d53.png)

显示模式改变。

让我们开始定制显示模式按钮项(如果你有更简单的方法，我很乐意知道！)，下面是代码。

显示模式代码。

1.  我们创建一个按钮，`[SplitViewControllerViewModel](https://gist.github.com/jamesrochabrun/1f02c6377ecd895e952075e70df21d04)`只是一个视图模型，提供不同状态的图标。
2.  我们构建了一个选择器来切换显示模式状态。
3.  在这里我们发现了一些有趣的东西，什么是`[displayMode](https://developer.apple.com/documentation/uikit/uisplitviewcontroller/1623194-displaymode)`，它与`prefferedDisplayMode`有什么不同？这也是一个`UISplitViewController`属性，但它只是一个 getter，代表你的视图控制器的当前排列。

> *该属性反映了拆分视图界面中两个子视图控制器的排列。该属性中的值永远不会设置为* `[*UISplitViewController.DisplayMode.automatic*](https://developer.apple.com/documentation/uikit/uisplitviewcontroller/displaymode/automatic)` *。要更改当前显示模式，请更改* `*preferredDisplayMode*` *属性的值。*

我们只是用它作为参考来确定当前的显示模式状态，然后切换`prefferedDisplayMode`，这就是诀窍所在。🤘

4.我们将在后面讨论更多关于`UISPlitViewControllerDelegate`的内容，但这里有一点要注意，每次你设置`prefferedDisplayMode`属性时，这个委托方法都会被触发。

 [## split view controller(_:willChangeTo:)

### 当拆分视图控制器的显示模式将要改变时，它调用此方法。因为改变…

developer.apple.com](https://developer.apple.com/documentation/uikit/uisplitviewcontrollerdelegate/1623176-splitviewcontroller) 

在这里我们可以通知子控制器的变化，这正是我们接下来要做的。

# 面向协议的用户界面更新。

现在，我们将为细节视图控制器添加填充，并按计划在顶部显示高亮 UI，我们需要一种方式将`SplitViewController`与`ContentDetailViewController`(当前的次级视图控制器)进行通信，我们可以使用通知、代理等。但是我们将使用面向协议的方法，这是代码…

1.  我们用两个方法创建了一个协议，我们将使用它们来触发符合它的子视图控制器的 UI 更新。
2.  我们有一个对拆分视图控制器的主视图控制器和次视图控制器的引用，因此我们可以使用它来获得一个符合`DisplayModeUpdatable`的`topViewController`，并执行将更新后的`displayMode`作为参数传递的方法。
3.  这里也一样，但是我们可以调用`displayModeWillChange`来代替并传递新的`displayMode`作为参数。
4.  如果你去`ContentDetailViewController` [文件](https://github.com/jamesrochabrun/InstagramIpad/blob/master/SplitViewControllerTutorial/ViewControllers/Details/ContentDetailViewcontroller.swift#L79)你可以看到它符合`DisplayModeUpdatable.`
5.  这正是添加/移除水平填充更新约束常数所需的逻辑。此外，您可以看到`verticalFeedTableView`，这是一个显示提要的可重用视图，也有一个`displayMode`属性，稍后将在步骤 6 中使用。
6.  highlights carrousel 只是`tableView`中的一个标题，它根据`displayMode`当前值设置其高度，使标题在适当的时候可见。

这就是你在这里所需要的，你可以拿这个例子，在`UISplitViewController`委托方法被触发时，实现孩子可以采用的小协议。

最后一件事，`UISplitViewController`有一个名为`[isCollapsed](https://developer.apple.com/documentation/uikit/uisplitviewcontroller/1623185-iscollapsed)`的属性，当它为真时，显示模式被忽略。

> *显示模式适用于扩展排列中的分割视图控制器。当拆分视图界面折叠时，显示模式被忽略。*

我们接下来会讲到。

# UISplitViewController 折叠

> *当分割视图控制器内容在语义上被折叠成单个容器时，该属性被设置为* `*true*` *。当拆分视图控制器从水平规则环境过渡到水平紧凑环境时，会发生折叠。折叠后，拆分视图控制器报告在其* `[*viewControllers*](https://developer.apple.com/documentation/uikit/uisplitviewcontroller/1623181-viewcontrollers)` *属性中只有一个子视图控制器。在委托对象的帮助下，另一个视图控制器被折叠到另一个视图控制器的内容中，或者被暂时丢弃。当* `*displayMode*` *属性折叠时，对拆分视图控制器界面的外观没有影响。*

下面是`isCollapsed`为真时 iPad 的样子…

![](img/ba0548e5774e2c20c6ce3bd72ebb67c6.png)

isCollapsed = true

在 iPad 开发过程中，这个话题让我大吃一惊🤯“拆分视图控制器的内容在语义上被折叠到一个容器中”是什么意思？这意味着当`isCollapsed`为真时，次视图控制器成为主视图控制器的子视图，两者都是导航控制器，等等，什么？导航控制器的子导航控制器？

![](img/2e3435d60d792c994a38cd0fd18bef14.png)

折叠时，次视图控制器作为主视图控制器的子视图。

等等，这不可能是正确的，这意味着最终我们将不得不把一个导航控制器放入一个导航控制器的堆栈中，但是这会抛出一个异常，对吗？🤔

> ***由于未捕获到异常“NSInvalidArgumentException”而终止应用程序，原因:“不支持推送导航控制器”***

看起来苹果在引擎盖下做了一个例外，当这发生在一个`UISplitViewController`中时，如果你在`willShow` `NavigationController`委托方法中放一个断点并打印`viewController`，你可以看到它实际上是在推动一个导航控制器。😳

![](img/750f43c6b022698cd87abe3222b33dd1.png)

好的，我想这很酷，看起来没有问题，但是 QA 开始玩这个应用程序了…😭

![](img/830581cd5bcb2f89eca8b6f40996ce8b.png)

推送和设备旋转时导航项目的错误

你看到问题了吗？密切注意导航栏，现在辅助视图控制器的导航项目显示为主视图控制器的导航项目，如果我们在 iOS 12 中这样做，应用程序甚至会显示一个空的子视图控制器的超级怪异的状态。我确信我不是一个人，我在推送和旋转的过程中发现了问题，甚至在苹果的应用程序中，比如文件应用程序。我花了一些时间试图找到这个问题的解决方案，但我仍然不确定这是否是最好的，但在阅读了我们在`UISPlitViewControllerDelegate` API 中可用的内容后，我找到了一个解决方法，现在让我们来谈谈这个委托。

# UISplitViewControllerDelegate

在查看了`UISplitViewControllerDelegate`文档后，我看到了这个方法，它的目的是在视图控制器以折叠模式“合并”后帮助执行“清理”。当分割视图控制器从`CompactWidth`扩展到`RegularWidth`使次视图可见时，它被触发。

 [## split view controller(_:separatesecordaryfrom:)

### 接口正在扩展的拆分视图控制器。主视图控制器…

developer.apple.com](https://developer.apple.com/documentation/uikit/uisplitviewcontrollerdelegate/1623189-splitviewcontroller) 

例如，在设备旋转时，当辅助设备像这样变得可见时…

![](img/aec7dded4ead97ec7ad8a1431fa7fe89.png)

当 SplitViewController 更改为常规宽度大小类时，设备旋转时第二个变得可见。

或者在这样的多任务环境中…

![](img/9003de96fe0dc1c903cba983d4664ccb.png)

当 SplitViewController 更改为常规 width size 类时，Secondary 在多任务环境中变得可见。

从文档中…

> *使用该方法为分割视图界面指定辅助视图控制器，并执行任何可能需要的额外清理。该方法返回后，分割视图控制器在其* `*viewControllers*` *阵列中安装新指定的主视图控制器和次视图控制器。*
> 
> *当一个界面折叠时，一些视图控制器合并主视图控制器和次视图控制器的内容。此方法是您撤销这些更改并将拆分视图界面返回到其原始状态的机会。*
> 
> *当你从这个方法返回* `*nil*` *时，分割视图控制器调用主视图控制器的* `*separateSecondaryViewController(for:)*` *方法，给它一个机会指定一个合适的次视图控制器。默认情况下，大多数视图控制器什么都不做，但是* `*UINavigationController*` *类通过从其导航堆栈的顶部弹出并返回视图控制器来做出响应。*

我意识到，在这里我也许可以做一些安排，使事情回到“正常”这里是代码，修复我们的问题，导航项目在 iOS 13 及以下…

```
**func** splitViewController(**_** splitViewController: UISplitViewController, separateSecondaryFrom primaryViewController: UIViewController) -> UIViewController? {**/// 1
if** **let** masterAsNavigation = primaryViewController **as**?   UINavigationController,**let** masterFirstChild = masterAsNavigation.viewControllers.first {/// 2
 masterAsNavigation.setViewControllers([masterFirstChild], animated: **false**)}
/// 3
**return** **nil**}
```

1.  好的，primaryViewController 是一个导航控制器，我们需要它的导航堆栈上的第一个 ViewController。
2.  在 primaryViewController 的数组中再次设置它……我知道这有点说不通，但是解决了这个问题。🤷‍♂️
3.  如果你阅读了文档，在这里返回 nil 是可以的，那么 splitViewController 会为你找到合适的视图控制器，至少在 iOS 13 中是这样的。

最后，iOS 12 中有一个我无法解决的 bug，它是设备旋转期间的弹出操作，这个异常看起来很熟悉吗？

```
child view controller:              THING I'M MOVING
should have parent view controller: SOURCE NAVIGATION CONTROLLER
but requested parent is:
```

我没有找到这方面的工作，所以如果你让我知道！这里有一个帖子实际上讲的是[问题](https://kerrick.wordpress.com/2017/06/29/fixing-_addchildviewcontrollerperformhierarchychecknotifywillmove/)。

现在让我们来谈谈当分割视图控制器从`RegularWidth`折叠到`CompactWidth`大小类时触发的委托方法。

 [## split view controller(_:collapse secondary:on:)

### 声明 splitViewController 界面正在折叠的拆分视图控制器。次要视图控制器…

developer.apple.com](https://developer.apple.com/documentation/uikit/uisplitviewcontrollerdelegate/1623184-splitviewcontroller) 

与最后一种方法相同，它也在设备旋转时触发，就像这样…

![](img/5ff1f65775e561910bdb8813aa4e9936.png)

当 splitViewController 采用紧凑宽度时，设备旋转时隐藏辅助。

或者在这样的多任务环境中…

![](img/0b251083a0c7e1a3b7e71ce3e98b45c8.png)

当 splitViewController 采用紧凑宽度时，辅助视图在多任务中隐藏。

那么，我们如何利用这种方法，我们的优势，以及折叠应用程序并启动它，你会看到这一点…

![](img/11ffa12a3c000096c08735c08fbf9c08.png)

默认情况下，“折叠启动”会显示详细信息。

如果没有内容，我们不希望默认显示细节，对吗？因此，要解决这个问题，您只需添加这段代码…

```
func splitViewController(_ splitViewController: UISplitViewController, collapseSecondary secondaryViewController: UIViewController, onto primaryViewController: UIViewController) -> Bool {
/// 1
return secondaryViewController is EmptyDetailViewController
}
```

1.  如果第二个是一个空的`EmptyDetailViewController`，这将返回 true，意味着用户没有执行选择，并且没有理由在折叠时显示这个 UI。

从[文档](https://developer.apple.com/documentation/uikit/uisplitviewcontrollerdelegate/1623184-splitviewcontroller) …

> 从这个方法返回 `*true*` *告诉分割视图控制器不要应用任何默认行为。如果您不想将辅助视图控制器的内容合并到结果界面中，可以返回* `*true*` *。*

你可以使用更多的`UISplitViewControllerDelegate`方法来满足你的需求，但我认为在这篇文章中讨论这些就足够了，如果你想看的话，这里有[个文件](https://developer.apple.com/documentation/uikit/uisplitviewcontrollerdelegate)。

同样，你可以在这里找到完整的代码[，你也可以在这里](https://github.com/jamesrochabrun/InstagramIpad)找到我[。](https://twitter.com/roch4brun)

干杯