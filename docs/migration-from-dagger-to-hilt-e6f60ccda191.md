# 从匕首转移到刀柄

> 原文：<https://blog.devgenius.io/migration-from-dagger-to-hilt-e6f60ccda191?source=collection_archive---------0----------------------->

## Android 依赖注入教程

## 让我们迁移到一个更好的地方！

![](img/48e36457720a0ba02a9f42e3dfd131b4.png)

图片由 [Goodheads](https://goodheads.io/2016/03/16/dependency-injection-explained-plain-english/) 提供

如果你有一个庞大的代码库要处理，而且已经有好几年了，而且其中有依赖注入，那么在用 Hilt 清理注入时，似乎很难弄脏你的手。最近，我们在 [**S** 英雄](https://sheroes.com/)中遇到了类似的情况，我们不得不从匕首到剑柄迁移我们的代码库。

在本文中，我们将看到我们如何在 [**SHEROES**](https://sheroes.com/) 应用中完全移植依赖注入。

# 目标状态

显然，我们不能在一开始就立即进行迁移，因为我们不希望我们的代码中断，而是在我们一步一步地迁移部分代码时继续工作。

我们的目标将按以下顺序排列

*   从所有类的注入中消除 Dagger 生成的 AppComponent 依赖
*   使 Activities/Fragments/Views 类成为我们通过 Hilt 注入的入口点，并从 Application 类中删除 AppComponent
*   我们将为不被 Hilt 直接支持的类提供入口点

# 移民

## 第一步

我们首先消除 dagger 单例组件的依赖性，并通过 Hilt 单例组件提供它们。这就是它在应用程序类中的呈现方式。

这个 AppComponent 类如下所示:

现在，我们首先为 app 组件创建另一个接口，名为 *AppModuleComponent* ，并通过 Hilt 组件提供单例模块。我们还用 *@EntryPoint* 和 *@InstallIn* 注释了旧的 *SheroesAppComponent* 接口，如下所示:

其他一切暂时保持不变。但是如果我们在这个时间点上构建这个项目，那么我们将会得到一个异常，声明由于 Hilt 的期望，所有的 Hilt 模块都应该提供 InstallIn。

在迁移完全完成之前，我们可以通过在应用程序 Gradle 文件中添加以下行来避免这种异常。

现在我们已经更新了从 *SheroesApplication* 类中的 dagger 获取应用组件对象的方式

从

```
mSheroesAppComponent = DaggerSheroesAppComponent.builder().build();
```

到

```
mSheroesAppComponent = EntryPointAccessors.*fromApplication*(*this*, SheroesAppComponent::*class*.java)
```

当我们在这个时间点构建我们的项目时，我们没有出现任何错误，一切都照常运行。我们已经从 Hilt 提供了我们的应用组件依赖。

从这里，我们现在进入下一步。

## 第二步

现在，我们开始用*@ Android identry point*标记我们的活动/片段/视图/非视图注入类，并通过这些类中的 app 组件移除注入。

对我们来说，一个被注入了匕首的活动如下

```
*class* HomeActivity: AppCompatActivity() {
    @override onCreate(bundle: Bundle) {
        SheroesApplication.getAppComponent(*this*).inject(*this*);
    }
}
```

我们用*@ Android identry point*注释了这个活动，并通过 app 组件移除了这个注入。

```
*@AndroidEntryPoint
class* HomeActivity: AppCompatActivity() { @Inject
    appUtils: AppUtils() @override onCreate(bundle: Bundle) {
        // no injection code required now
    }
}
```

我们在所有的视图类中一步一步地这样做，并在每个阶段验证它。然后我们也从 *SheroesAppComponent* 接口中移除了相同的注入类。

这样，我们从所有视图类中消除了应用程序组件注入。

## 第三步

现在开始支持那些没有被 Hilt 直接支持的类。例如，util 类、视图容器、适配器等等。

我们有使用 Dagger 注入的视图持有者和工具类。那么如何为这样的类提供依赖呢？希尔特也有办法做到这一点。

作为参考，考虑以下带有 Dagger 注入的 ViewHolder 类。

为了给这些类提供依赖关系，我们创建了一个入口点接口，并定义了这个类需要的所有依赖关系。对于上述视图持有者类别，它需要用户偏好。因此，我们的入口点将如下所示

现在，我们通过这个入口点向视图持有者提供依赖，如下所示

Bamn！依赖项已注入。建立了项目，一切都像以前一样好。

这里还要注意，我们从 activity reference 访问我们的入口点，因为我们将它安装在 *ActivityComponent* 中。所以我们需要确定我们安装的入口点是哪个组件，我们应该从同一个引用中提取它。

## 第四步

我们还需要确保我们现有的所有模块，如网络模块、数据库模块等也应该安装在使用 *@InstallIn* 注释的 Hilt 组件中。这样，我们几乎已经完成了迁移，只剩下最后一步了。

## 第五步

是时候清理残渣了。我们删除了

*   旧 AppComponent 接口
*   来自我们的应用程序类的 AppComponent 引用
*   app gradle 中的编译警告行

最后，构建了项目，一切都像以前一样工作。但是一定要确保运行和测试受迁移影响的应用程序的每个部分，因为如果任何依赖项丢失或没有正确提供，Hilt 将抛出运行时异常。

是的。我们已经把我们的 [SHEROES](https://sheroes.com/) 应用程序的代码从匕首移植到了刀柄。您可能在您的项目中有不同的设置，但是您可以在短时间内了解如何在不破坏现有流程的情况下完成它。

如果你有任何现有的匕首项目，那么是时候使用刀柄了。

# 目前就这些了！敬请期待！

与我联系(如果内容对您有帮助),请访问

*   [中等](https://saurabhpant.medium.com/)
*   [GitHub](https://github.com/aqua30)
*   [推特](https://twitter.com/saurabh30pant)
*   [领英](https://www.linkedin.com/in/saurabh-pant-44619057/)

订阅电子邮件，同步了解更多关于 Android/IOS/Backend/Web 的有趣话题。

直到下一次…

干杯！