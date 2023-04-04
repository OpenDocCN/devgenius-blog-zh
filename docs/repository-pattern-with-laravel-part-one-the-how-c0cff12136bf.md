# Laravel 的存储库模式(第一部分):如何做！

> 原文：<https://blog.devgenius.io/repository-pattern-with-laravel-part-one-the-how-c0cff12136bf?source=collection_archive---------3----------------------->

![](img/38599902cd220075480b863cb41beac3.png)

穆罕默德·拉赫马尼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

设计模式已经存在了很长时间，不仅帮助我们维护代码，还帮助我们提高灵活性、可测试性、可重用性等等。其中一种设计模式是**存储库模式**，这种模式已经在 Laravel 社区中流行了好几年了。

考虑到我们大多数人对“为什么”使用设计模式有一些了解，以及谈论它的“为什么”方面可能有多广泛，我们将在未来的帖子中直接深入 Laravel 的存储库模式的实现，并讨论 Laravel 对该模式的需求。

**注意:我们这里的示例项目将是一个简单的 Todo 应用程序。**

考虑带有 index 和 show 方法的 TodosController，如下所示:

```
<?phpuse App\Todo;class TodosController extends Controller
{
  public function index()
  {
    $todos = Todo::where('status','0')->get();
    return view('todos.index',compact('todos'));
  } public function show($id)
  {
    $todo = Todo::findOrFail($id);
    return view('todos.show',compact('todo'));
  }
} 
```

***第一步:*** 创建必要的文件

在应用程序目录中创建一个文件夹，在其中创建另一个名为“Todo”的文件夹(通常是您的模型名称)。添加两个新文件一个接口(比如 TodoInterface.php)和一个存储库类(比如 TodoRepository.php)。

**这是界面的样子。**

```
<?phpnamespace ... ;interface TodosInterface
{
  public function getTodo(); //these method names are custom
  public function showTodo($id); //these method names are custom }
```

***第三步:*** 在 repository 类中定义前面提到的自定义方法如下。

```
<?phpnamespace ... ;
use App\Todo;class TodoRepository implements TodoInterface 
{
  public function getTodo()
  {
    return Todo::all();
  } public function showTodo($id)
  {
    return Todo::findOrFail($id);
  }}
```

***第四步:*** 绑定:我们需要将存储库类与存储库接口绑定，然后它们才能在我们的应用程序上运行。为所有存储库类绑定创建一个服务提供者可能是一个更好的选择。但是对于 Todo list 这样的小应用程序，我们可以使用 laravel 的 AppServiceProvider 类

```
public function register()
{
 $this->app->bind(TodoInterface::class,TodoRepository::class);
}
```

***第五步:*** 依赖注入到我们的 TodosController 中。现在我们的控制器看起来像这样。

```
<?phpuse App\Todo;
user TodoInterface;class TodosController extends Controller
{ public function __construct(TodoInterface $todo)
  {
    $this->todo = $todo;
  } public function index()
  {
    $todos = $this->todo->getData();
    return view('todos.index',compact('todos'));
  }public function show($id)
  {
    $todo = $this->todo->showTodo($id);
    return view('todos.show',compact('todo'));
  }
}
```

**结论**

到目前为止，虽然控制器上只更新了几行代码，而且控制器看起来可能还是一样的，但它已经带来的好处是前所未有的。对于一个规模大得多的应用程序来说，使用和不使用这种设计模式的项目之间的差异可以从视觉上和逻辑上看出来。

在接下来的文章中，我们肯定会分解使用存储库模式的好处。在此之前，无论应用程序的规模如何，都可以随意使用这种模式。