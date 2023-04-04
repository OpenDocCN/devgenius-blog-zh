# Laravel 存储库设计模式-(快速演示)

> 原文：<https://blog.devgenius.io/laravel-repository-design-pattern-a-quick-demonstration-5698d7ce7e1f?source=collection_archive---------0----------------------->

![](img/651851d402dc78bfb9130448001ec007.png)

穆罕默德·拉赫马尼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

存储库设计模式是对应用程序数据层的抽象。存储库可以被描述为域和数据映射层之间的一个抽象层，它通过一个访问域对象的类似集合的接口提供了两者之间的一个中介。

## **先决条件**

本教程将是一个实践演示。如果你想继续学习，请确保你具备以下条件:对 PHP 和 [Laravel](https://laravel.com/docs/9.x) 、
有基本的了解，并且你已经安装了 PHP 7.4 和 [Laravel 安装程序](https://laravel.com/docs/8.x/installation#the-laravel-installer)。 [Composer](https://getcomposer.org/) 全球安装。

## 入门指南

在本教程中，我们将构建一个简单的应用程序来演示如何使用 Laravel 中的存储库设计模式。我们将首先为一个演示建立一个任务管理系统，我们需要通过 Laravel 安装程序安装 Laravel。
在您的终端上运行以下代码。

```
**laravel new task_manager**
```

> 注意:确保您已经在电脑中安装并设置了 [Laravel 安装程序](https://laravel.com/docs/8.x/installation#the-laravel-installer)。

安装完成后，接下来我们使用终端导航到刚刚创建的任务管理器。在您的终端中运行以下代码。

```
**cd task_manager**
```

## 设置数据库

我们需要设置我们希望在这个应用程序中使用的数据库。Laravel 提供了一种简单的方法来做到这一点。为此，在。env 文件，更新与数据库相关的参数，如下所示。

最后，使用您首选的数据库管理应用程序，创建一个名为 task_manager 的新数据库。

## 生成**模型、迁移和控制器**

在 Laravel 中，生成模型迁移和控制器非常简单，您只需在终端中运行以下命令。

```
**php artisan make:model Task -mc**
```

-mc 参数让 Artisan 知道我们想要为任务模型创建一个[迁移](https://laravel.com/docs/8.x/migrations#introduction)文件和控制器。

上面的命令将创建三个新文件:

*   *app/Http/Controllers/task controller . PHP*中的一个控制器
*   *数据库/migrations/YYYY _ MM _ DD _ HHMMSS _ create _ tasks _ table . PHP*中的一个迁移文件
*   位于 *app/Models/Task.php* 中的模型

在*数据库/迁移/YYYY _ MM _ DD _ HHMMSS _ create _ tasks _ table . PHP*中，更新`up`函数以匹配以下内容。

如迁移文件中所指定的，`tasks`表将包含以下各列:

1.  身份证。这将是表的主键。
2.  任务的标题。
3.  任务的详细描述
4.  任务是否已经完成。
5.  当任务被创建和更新时，`created_at`和`updated_at`，由`timestamps`函数提供。

接下来，我们运行以下命令来迁移我们刚刚创建的迁移

```
**php artisan migrate**
```

## 构建前端

我们将使用 HTML 和 CSS (Bootstrap)构建这个任务管理应用程序的前端。首先，让我们创建登录和注册页面。打开***resources/view****文件夹，创建以下文件夹，在 layouts 文件夹内 t **asks** ， **layouts，auth** 创建***app.blade.php***文件，你现在有***resources/view/layouts/app . blade . PHP .****

*在任务里面，文件夹创建了以下文件，***index.blade.php，create.blade.php show.blade.php，edit.blade.php。****

*打开***resources/view/layouts/app . blade . PHP***，粘贴以下代码。*

*打开***resources/view/tasks/index . blade . PHP***，粘贴以下代码。*

*打开***resources/view/tasks/create . blade . PHP***，粘贴以下代码。*

*打开***resources/view/tasks/show . blade . PHP***粘贴以下代码。*

*打开***resources/view/tasks/edit . blade . PHP***，粘贴以下代码。*

> ***注意**:你可以随心所欲地设计前端，本教程的重点不是前端，而是存储库设计模式*

## *创建存储库接口*

*在为`Task`模型创建存储库之前，让我们定义一个接口来指定存储库必须声明的所有方法。我们的控制器(以及我们将来可能构建的任何订单组件)将依赖于接口，而不是直接依赖于存储库类。
这是存储库设计模式的众多好处之一，它将使我们的代码更加灵活，因为如果将来有必要进行更改，控制器将不受影响。
例如，如果我们决定将任务管理外包给第三方应用程序，或者我们想将数据库系统更改为 Laravel inventory 不支持的另一个数据库，我们可以构建一个符合`TaskRepositoryInterface`签名的新模块并交换绑定声明，我们的控制器将完全按预期工作——而无需接触控制器中的任何一行代码。*

*在 *app* 目录中，创建一个名为 *Interfaces* 的新文件夹。然后，在*界面*中，创建一个名为*TaskRepositoryInterface.php*的新文件，并添加以下代码。*

## *创建存储库*

*接下来，在 app 文件夹中，创建一个名为“Repositories”的新文件夹。在此文件夹中，创建一个名为*TaskRepository.php*的新文件，并向其中添加以下代码。*

*除了接口提供的灵活性之外，以这种方式封装查询还有一个额外的优势，即我们不必在整个应用程序中重复查询。
如果将来我们决定只检索`getAllTasks()`函数中未完成的任务，我们只需要在一个地方做一个改变，而不是追踪`Task::where('user_id', auth()->user()->id)`声明的所有地方，同时冒丢失一些的风险*

*在 [Laravel](https://laravel.com/docs/8.x#meet-laravel) 应用程序中使用存储库设计模式的主要思想是在模型和控制器之间创建一个桥梁。换句话说，就是将模型的硬依赖从控制器中去耦合。模型不应负责与数据库通信或从数据库中提取数据。模型应该是表示给定表/文档/对象或数据结构中的任何其他类型的对象，这应该是它的唯一责任。因此，为了让你的 **Laravel 代码保持干净和安全**，使用存储库来区分模型永远不应该负责的责任是值得的。*

## *创建控制器*

*现在我们已经创建了存储库，让我们向控制器添加一些代码。打开*app/Http/controller/task controller . PHP*并更新代码以匹配以下内容。*

*代码通过构造函数注入一个`TaskRepositoryInterface`实例，并在每个控制器方法中使用相关对象的方法。*

*首先，在`index()`方法中，它调用定义了`taskRepository`的`getAllTasks()`方法来检索任务列表并在视图中返回。*

*接下来，`store()`方法从`textRepository`调用`createTask()` *方法*来创建一个新任务。这将需要创建的任务的细节作为一个数组，然后返回一个成功的响应。*

*在控制器的`show()`方法中，它从路由中检索唯一的任务`Id`，并将其作为参数传递给`getTaskById()`。这将从数据库中获取带有匹配 Id 的任务的详细信息，并以 JSON 格式返回一个响应。*

*然后，为了更新已经创建的任务的细节，它从存储库中调用`updateTask()`方法。这需要两个参数:任务的惟一 id 和需要更新的细节。*

*最后，`destroy()`方法从路由中检索特定任务的惟一 id，并从存储库中调用`deleteTask()`方法来删除它。*

## *添加路线*

*要将控制器中定义的每个方法映射到特定的路由，请将以下代码添加到 *routes/web.php* 中。*

> *记住包括`TaskController`的`import`语句。*

```
*use App\Http\Controllers\TaskController;*
```

## *绑定接口和实现*

*我们需要做的最后一件事是将`TaskRepository`绑定到 [Laravel 的服务容器](https://laravel.com/docs/8.x/container#introduction)中的`TaskRepositoryInterface`；我们通过[服务提供商](https://laravel.com/docs/8.x/providers)来完成这项工作。使用以下命令创建一个。*

```
*php artisan make:provider RepositoryServiceProvider*
```

*打开*app/Providers/repositoryserviceprovider . PHP*并更新`register`函数以匹配以下内容。*

```
*public function register() 
{
    $this->app->bind(TaskRepositoryInterface::class, TaskRepository::class);
 }*
```

*记住包括`TaskRepository`和`TaskRepositoryInterface`的`import`声明。*

```
*use App\Interfaces\TaskRepositoryInterface;
use App\Repositories\TaskRepository;*
```

*最后，将新的服务提供者添加到 *config/app.php.* 中的`providers`数组*

```
*'providers' => [
    // ...other declared providers
    App\Providers\RepositoryServiceProvider::class,
];*
```

## *测试应用程序*

*使用以下命令运行应用程序。*

```
*php artisan serve*
```

*默认情况下，提供的应用程序将在 [http://127.0.0.1:8000](http://127.0.0.1:8000/) 可用。*

> *关于存储库设计模式的教程已经接近尾声。这是一个关于如何在你的项目中实现存储库设计模式的简短演示。我们学习了存储库模式以及如何在 Laravel 应用程序中使用它。我们也看到了它为大规模项目提供的一些好处——其中之一是松散耦合的代码，我们编写抽象的代码，而不是具体的实现。
> 最后，总结一下实现存储库模式时需要注意的事项。*
> 
> *每个存储库都应该实现自己的接口！不要在没有实现自己的接口的情况下创建存储库，也不要为所有的存储库创建一个接口。这不是好的做法！*
> 
> *如果您总是使用依赖注入来注入您的存储库，而不是使用 new 关键字来创建实例，这将会有所帮助。作为类型，总是使用接口，而不是实现！如果你把对象注入到类中，编写单元测试会更容易，你是在警告 SRP(单一责任原则)，代码看起来也更干净。*
> 
> *让你的代码可重用。如果不止一个存储库使用任何方法，您应该将该方法实现到 BaseRepository 类中，以避免 DRY 原则。*
> 
> *在你的存储库中，将模型注入到一个构造函数中，不要使用一个类作为静态的。通过这样做，您可以很容易地在单元测试中模仿您的模型！*

## *结论*

*然而，我将在结束发言时提出警告。对于小项目来说，这种方法会让人觉得有很多工作和样板代码，可能不会立即显现出来。因此，在采用这种方法之前，适当地考虑项目的规模是很重要的。*