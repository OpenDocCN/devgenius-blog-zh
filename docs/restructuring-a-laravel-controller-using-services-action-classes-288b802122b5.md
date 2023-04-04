# 使用服务和动作类重构 Laravel 控制器

> 原文：<https://blog.devgenius.io/restructuring-a-laravel-controller-using-services-action-classes-288b802122b5?source=collection_archive---------0----------------------->

## Laravel 重构——Laravel 从头开始创建管理面板——第 11 部分

![](img/a013316adbc58256fe4805a67517e35b.png)

照片由 [GR Stocks](https://unsplash.com/@grstocks?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在[前一部分](/restructuring-a-laravel-controller-using-form-request-validation-e546fe22c298)中，我们将`UserController` `store`方法验证移至表单请求。在这一部分中，我们将探索和使用新的趋势操作和服务类。

我们将在博客中讨论以下话题

*   Laravel 项目结构
*   控制器重构
*   服务级别
*   —什么是服务等级
*   —实施服务类别
*   动作类
*   —实施行动类别
*   服务和行动类别的优势
*   服务和行动类别的缺点
*   结论

# Laravel 项目结构

Laravel 不限制你的项目结构，也不建议任何项目结构。所以，你可以自由选择你的项目结构。

> Laravel 为您提供了自行选择结构的灵活性

我们将探索服务和动作类，并在我们的 [Laravel 基本管理面板](https://github.com/balajidharma/basic-laravel-admin-panel)中使用这些类。

# 控制器重构

`UserController`商店功能完成以下 3 个动作。

```
public function store(StoreUserRequest  $request)
{
    // 1.Create a user
    $user = User::create([
        'name' => $request->name,
        'email' => $request->email,
        'password' => Hash::make($request->password),
    ]);
    // 2.Assign role to user
    if(! empty($request->roles)) {
        $user->assignRole($request->roles);
    }
    // 3.Redirect with message
    return redirect()->route('user.index')
                    ->with('message','User created successfully.');
}
```

为了进一步重构，我们可以将逻辑移到另一个类方法中。这个新类叫做服务和动作类。我们将一个一个地看到他们。

[](/laravel-works-with-large-database-records-using-the-chunk-method-9b69f41264da) [## Laravel 使用 chunk 方法处理大型数据库记录

### Laravel 分块数据库查询结果

blog.devgenius.io](/laravel-works-with-large-database-records-using-the-chunk-method-9b69f41264da) 

# 服务类别

我们决定将逻辑转移到另一个类。由于 [*单一责任原则*](https://en.wikipedia.org/wiki/Single-responsibility_principle) *(SRP)，建议使用 [*Laravel 最佳实践*](https://github.com/alexeymezenin/laravel-best-practices/#business-logic-should-be-in-service-class) 将业务逻辑从控制器转移到服务类。*Service 类只是一个普通的 PHP 类，用来放置我们所有的逻辑。

## 什么是服务等级

服务是一个非常简单的类，它没有用任何类来扩展。所以，它只是一个独立的 PHP 类。

我们将使用`createUser`方法创建一个新的`app/Services/Admin/UserService.php`服务类。这是 Laravel 中的一个自定义 PHP 类，所以没有 artisan 命令。我们需要手动创建它。

## 实现服务类别

`app/Services/Admin/UserService.php`

```
<?php
namespace App\Services\Admin;

use App\Models\User;
use Illuminate\Support\Facades\Hash;

class UserService
{
    public function createUser($data): User
    {
        $user = User::create([
            'name' => $data->name,
            'email' => $data->email,
            'password' => Hash::make($data->password),
        ]);

        if(! empty($data->roles)) {
            $user->assignRole($data->roles);
        }

        return $user;
    }
}
```

然后，在 UserController 中调用这个方法。对于[自动注射](https://laravel.com/docs/9.x/container#automatic-injection)，您可以在控制器中键入提示依赖关系。

**博客更新:**之前我将$request ( `function createUser(Request $request)`)直接传递给了服务类。该服务可以通过其他方法使用。所以$request 被转换成一个对象并作为参数传递。

`app/Http/Controllers/Admin/UserController.php`

```
use App\Services\Admin\UserService;
public function store(StoreUserRequest $request, UserService $userService)
{
    $userService->createUser((object) $request->all());
    return redirect()->route('user.index')
                    ->with('message','User created successfully.');
}
```

我们可以通过将用户角色保存移动到新方法中来对 UserService 类进行更多的重构。

`app/Services/Admin/UserService.php`

```
class UserService
{
    public function createUser($data): User
    {
        $user = User::create([
            'name' => $data->name,
            'email' => $data->email,
            'password' => Hash::make($data->password),
        ]);
        return $user;
    }
    public function assignRole($data, User $user): void
    {
        $roles = $data->roles ?? [];
        $user->assignRole($roles);
    }
}
```

`app/Http/Controllers/Admin/UserController.php`

```
public function store(StoreUserRequest $request, UserService $userService)
{
    $data = (object) $request->all();
    $user = $userService->createUser($data);
    $userService->assignRole($data, $user);
    return redirect()->route('user.index')
                    ->with('message','User created successfully.');
}
```

现在我们实现了服务类。我们将在博客的最后讨论它的好处。

[点击此处](https://laravelexamples.com/tag/services)查看 Laravel 上使用的服务等级示例

# 动作类

在 Laravel 社区，动作类的概念近年来变得非常流行。动作是一个非常简单的 PHP 类，类似于服务类。但是 Action 类只有一个公共方法`execute`或`handle`，否则你可以随意命名该方法。

## 实现操作类

我们将使用单个`handle`方法创建一个新的`app/Actions/Admin/User/CreateUser.php`动作类。

`app/Actions/Admin/User/CreateUser.php`

```
<?php

namespace App\Actions\Admin\User;

use App\Models\User;
use Illuminate\Support\Facades\Hash;

class CreateUser
{
    public function handle($data): User
    {
        $user = User::create([
            'name' => $data->name,
            'email' => $data->email,
            'password' => Hash::make($data->password),
        ]);

        $roles = $data->roles ?? [];
        $user->assignRole($roles);

        return $user;
    }
}
```

现在在 UserController 上调用这个句柄方法。[法注射](https://laravel.com/docs/9.x/container#automatic-injection)来化解`CreateUser`。

`app/Http/Controllers/Admin/UserController.php`

```
public function store(StoreUserRequest $request, CreateUser $createUser)
{
    $createUser->handle((object) $request->all());
    return redirect()->route('user.index')
                    ->with('message','User created successfully.');
}
```

这个 Action 类的最大优点是我们不用担心函数名。因为它应该总是像`handle`一样的单一功能

# 服务和行动类别的优势

*   **代码可重用性**:我们可以调用 Artisan 命令上的方法，也很容易调用其他控制器。
*   **单一责任原则(SRP)** :通过使用服务&动作类实现 SRP
*   **避免冲突**:易于管理大型开发团队的大型应用程序的代码。

# 服务和行动类别的缺点

*   **太多的类**:我们需要为单一功能创建太多的类
*   **小型应用**:不推荐用于小型应用

# 结论

如前所述，Laravel 为您提供了自行选择结构的灵活性。服务和动作类是结构方法之一。对于大规模的应用程序，应该推荐使用它，以避免冲突和更快的发布。

对于 [Laravel 基本管理面板](https://github.com/balajidharma/basic-laravel-admin-panel)，我将使用 Actions 类。

Laravel 管理面板可在[https://github.com/balajidharma/basic-laravel-admin-panel](https://github.com/balajidharma/basic-laravel-admin-panel)获得。安装管理面板并分享您的反馈。

感谢您的阅读。

敬请关注更多内容！

*跟我来*[***balajidharma.medium.com***](https://balajidharma.medium.com/)。

**参考文献**

```
[https://freek.dev/1371-refactoring-to-actions](https://freek.dev/1371-refactoring-to-actions) [https://laravel-news.com/controller-refactor](https://laravel-news.com/controller-refactor)
[https://farhan.dev/tutorial/laravel-service-classes-explained/](https://farhan.dev/tutorial/laravel-service-classes-explained/)
```

上一部分—第 10 部分:[使用表单请求验证重构 Laravel 控制器](/restructuring-a-laravel-controller-using-form-request-validation-e546fe22c298)

下一部分—第 12 部分:[在 Laravel 中创建可重用的刀片组件](/create-reusable-blade-components-in-laravel-5607d8f306a6)