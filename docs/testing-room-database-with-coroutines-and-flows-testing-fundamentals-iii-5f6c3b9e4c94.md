# 使用协程和流程测试房间数据库—测试基础

> 原文：<https://blog.devgenius.io/testing-room-database-with-coroutines-and-flows-testing-fundamentals-iii-5f6c3b9e4c94?source=collection_archive---------2----------------------->

## Android 测试教程

## 永远为流动的变化留有空间！

![](img/fe4330cc7ca5ff4cbb94b2f5ff44f2d8.png)

来源:https://testinggenez.com/

我们大部分时间处理的一个常见用例是在我们的应用程序中使用本地数据库来支持缓存，为此使用空间是众所周知的。因此，为我们的数据库编写测试变得非常重要，因为在许多情况下，本地数据库是应用程序的唯一数据源。

使用数据库最常见的操作是 CRUD 操作。因此，在本文中，我们将探索如何为这样的操作编写测试，以及当它们返回给我们一个流时。

我们将有一个简单的应用程序，以名称的项目，并将其保存在数据库中。让我们开始吧。

# 房间布置

首先，我们需要在应用程序中设置房间数据库。所以让我们快速地做那件事。如下所示添加依赖项。

现在创建一个表实体，如下所示。

然后如下定义 Dao 接口。这是我们定义数据库 API 的类。我们的添加和删除功能被暂停。

然后创建数据库实体，如下所示。

就是这样。设置完成了。现在，我们可以快速浏览一下刚刚设置的内容。因此，我们为数据库定义了三个简单的 API，分别是添加/更新项目、删除项目和获取所有项目。

# 测试设置

为了测试我们的数据库设置，我们需要事先了解以下几点

*   在测试环境中，我们不是在设备存储中拥有真实的数据库，而是在内存中创建一个数据库，或者你可以说是一个临时数据库，以便我们可以快速测试它，并且它不会给我们的设备或应用程序增加任何额外的负载。一旦测试完成，它就会被移除。
*   因为我们使用流来观察任何数据库变化，所以在测试环境中观察流是很棘手的。所以我们将使用一个名为[涡轮机](https://github.com/cashapp/turbine)的库。它使得测试流程变得容易。我们需要在 androidTestImplementation 下添加它的依赖项，如下所示

```
androidTestImplementation 'app.cash.turbine:turbine:0.9.0'
```

*   由于 Room 是一个 android 库，我们将在 androidTest 目录中编写测试。

让我们为 ItemDao 创建测试类。

我们在这个类上添加了两个注释。@ ***RunWith*** 注释告诉编译器用 AndroidJUnit4 而不是 JUnit apis 运行这个类。

@ ***SmallTest*** 注释告诉编译器，它正在频繁地测试运行，它的执行时间应该小于 200ms。测试大小限定符是构建测试代码的一种很好的方式，用于将一个测试分配给一个运行时间相似的测试套件。

现在我们需要将我们的协程调度程序从 main 改为 test dispatcher，否则我们的测试将会异常失败。为此，我们创建一个规则，简单地将主调度程序设置为测试调度程序，并在完成时重置它。

我们将这个规则添加到 ItemDaoTest.kt 中。

```
@get: Rule
*val* dispatcherRule = TestDispatcherRule()
```

现在让我们为测试创建数据库和 dao 实例。

如前所述，我们在注释前的**下创建了一个内存中的数据库实例，并在**注释后关闭了**中的数据库。**

好吧！终于到了写测试的时候了。我们将编写以下测试用例

*   在数据库中添加两个项目并获取所有项目流应该返回这两个项目。
*   添加两个项目并删除其中任何一个。我们的获取所有项目流应该只返回未删除的项目。
*   添加两项并更新其中任何一项。我们的 get all items 流应该返回更新的项目。

# 编写测试用例

## 测试案例 1

在数据库中添加一个项目，应该在我们的流程中返回。

```
@Test
*fun* addItem_shouldReturn_theItem_inFlow() = *runTest* **{ }**
```

因为我们的 add 函数是一个 suspend 函数，我们需要在测试协程生成器 ***runTest 下运行它。***

然后，我们创建我们的项目，并将它们添加到数据库中。

```
*val* item1 = Item(uid = 1, itemName = "item 1")
*val* item2 = Item(uid = 2, itemName = "item 2")
itemDao.addItem(item1)
itemDao.addItem(item2)
```

现在我们使用扩展函数 ***test 来测试我们的流。*** 我们使用汽轮机的 ***awaitItem()*** 函数观察发射项列表。

```
itemDao.getAllItems().test **{** *val* list = awaitItem()
    *assert*(list.contains(item1))
    *assert*(list.contains(item2))
    cancel()
**}**
```

一旦我们有了列表，我们就断言这两个项目是否都在列表中。如果是，那么测试将通过，否则将失败。

最后我们调用 ***cancel()*** 函数，再次从涡轮库中取出。

> 这是一个重要的捕捉，因为如果我们不进行这个调用，那么我们的流将永远不会完成，测试将永远运行下去。在测试环境中，流应该是有限的，只有这样它才能完成，我们才能在发出的值上测试我们的断言。

运行上述测试的结果将是

![](img/fce5e1eb52619fb00b97ee1ff3312137.png)

太好了！因此，我们已经成功地执行并测试了我们的第一个测试。现在让我们测试从数据库中删除项目。

## 测试案例 2

在数据库中删除一个项目，应该在我们的流程中返回。

```
@Test
*fun* deletedItem_shouldNot_be_present_inFlow() = *runTest* **{ }**
```

为了执行这个测试，我们将首先在数据库中添加两个项目，如下所示

```
*val* item1 = Item(uid = 1, itemName = "item 1")
*val* item2 = Item(uid = 2, itemName = "item 2")
itemDao.addItem(item1)
itemDao.addItem(item2)
```

然后，我们将对第二个项目执行删除操作(两者都可以)。

```
itemDao.removeItem(item2)
```

现在，我们观察这个流，并检查被删除的条目是否不在返回的条目列表中，如下所示

```
itemDao.getAllItems().test **{** *val* list = awaitItem()
    *assert*(list.size == 1)
    *assert*(list.contains(item1))
    cancel()
**}**
```

一旦我们完成断言，请确保调用 cancel。

运行测试，结果如下

![](img/5934496ae096ed7fed562ac55c18c9ea.png)

还有 Bamn！我们成功地测试了删除数据库中的项目。

## 测试案例 3

更新数据库中的项目，应该在我们的流程中返回。

```
@Test
*fun* updateItem_shouldReturn_theItem_inFlow() = *runTest* **{ }**
```

> 如果您已经了解了这么多，那么您一定已经对如何测试我们的数据库以及测试什么有了足够的了解。所以现在您可以自己编写更新项目测试用例并断言条件。
> 
> 请发表评论，让我知道你是否能够成功地做到这一点，即使没有。🤘

哇哦。我们已经为我们的房间数据库编写了测试用例，并测试了我们的流程。这将足以开始测试，并使您的代码更加防错。现在进行设置，并尝试不同的场景。

您可以查看下面的完整要点以供参考。

# 有奖阅读

*   [视图模型测试](/writing-viewmodel-tests-in-android-testing-fundamentals-ii-5bc44efa4a39)
*   [如何在 Android 中编写本地测试](/writing-viewmodel-tests-in-android-testing-fundamentals-ii-5bc44efa4a39)

# 目前就这些了！敬请期待！

与我联系(如果内容对您有帮助),请访问

*   [中等](https://saurabhpant.medium.com/)
*   [GitHub](https://github.com/aqua30)
*   [推特](https://twitter.com/saurabh30pant)
*   [领英](https://www.linkedin.com/in/saurabh-pant-44619057/)

订阅电子邮件，同步了解更多关于 Android/IOS/Backend/Web 的有趣话题。

直到下一次…

干杯！