# 使用表单请求验证重构 Laravel 控制器

> 原文：<https://blog.devgenius.io/restructuring-a-laravel-controller-using-form-request-validation-e546fe22c298?source=collection_archive---------7----------------------->

## Laravel 重构——Laravel 从头开始创建管理面板——第 10 部分

![](img/193b1c3321086dbad2c2a94fa86f6b24.png)

Julia Solonina 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 代码重构

[基本 Laravel 管理面板](https://github.com/balajidharma/basic-laravel-admin-panel)在最初发布时已经完成。在这一部分，我们将重组我们的控制器。

代码重构是重构现有代码的重要过程。

> 重构旨在改进应用程序的设计、结构和/或实现

# 控制器验证

Laravel 控制器从基本控制器扩展而来。这个基本控制器已经继承了 [ValidatesRequests 特征](https://github.com/laravel/laravel/blob/master/app/Http/Controllers/Controller.php#L12)。因此 ValidatesRequests 特征让您可以访问我们的控制器方法中的 validate 方法。

我们在下面的用户存储方法中使用验证方法

`app/Http/Controllers/Admin/UserController.php`

```
public function store(Request $request)
{
    $request->validate([
        'name' => ['required', 'string', 'max:255'],
        'email' => ['required', 'string', 'email', 'max:255', 'unique:users'],
        'password' => ['required', 'confirmed', Rules\Password::defaults()],
    ]);
    $user = User::create([
        'name' => $request->name,
        'email' => $request->email,
        'password' => Hash::make($request->password),
    ]);
    if(! empty($request->roles)) {
        $user->assignRole($request->roles);
    }
    return redirect()->route('user.index')
                    ->with('message','User created successfully.');
}
```

# 控制器重构

`serController`存储功能执行以下 4 个动作。

```
public function store(Request $request)
{
    // validate from request // Create a user // Assign role to user // Redirect 
}
```

> [单一责任原则](https://en.wikipedia.org/wiki/Single-responsibility_principle) (SRP)类或函数应该对程序功能的单一部分负责，并且应该封装该部分。

让我们在控制器中实现单一责任原则。首先，我们通过使用 Laravel [表单请求验证](https://laravel.com/docs/validation#form-request-validation)来分离我们的验证

# 表单请求验证

Latavel 表单请求是定制的请求类，封装了它们自己的验证和授权逻辑。要创建表单请求类，使用 Artisan CLI `make:request`命令

```
sail artisan make:request Admin/StoreUserRequest
```

Artisan CLI 创建了 StoreUserRequest 类。打开该类并复制验证规则

`app/Http/Requests/Admin/StoreUserRequest.php`

```
<?php
namespace App\Http\Requests\Admin;
use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Validation\Rules;
class StoreUserRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }
    /**
     * Get the validation rules that apply to the request.
     *
     * @return array<string, mixed>
     */
    public function rules()
    {
        return [
            'name' => ['required', 'string', 'max:255'],
            'email' => ['required', 'string', 'email', 'max:255', 'unique:users'],
            'password' => ['required', 'confirmed', Rules\Password::defaults()],
        ];
    }
}
```

现在打开用户控制器，进行如下更改

```
+++ b/app/Http/Controllers/Admin/UserController.php
@@ -3,6 +3,7 @@
 namespace App\Http\Controllers\Admin;
 use App\Http\Controllers\Controller;
+use App\Http\Requests\Admin\StoreUserRequest;
 use App\Models\User;
 use App\Models\Role;
 use Illuminate\Http\Request;
@@ -63,17 +64,11 @@ class UserController extends Controller
     /**
      * Store a newly created resource in storage.
      *
-     * @param  \Illuminate\Http\Request  $request
+     * @param  \App\Http\Requests\Admin\StoreUserRequest  $request
      * @return \Illuminate\Http\Response
      */
-    public function store(Request $request)
+    public function store(StoreUserRequest  $request)
     {
-        $request->validate([
-            'name' => ['required', 'string', 'max:255'],
-            'email' => ['required', 'string', 'email', 'max:255', 'unique:users'],
-            'password' => ['required', 'confirmed', Rules\Password::defaults()],
-        ]);
-
         $user = User::create([
             'name' => $request->name,
             'email' => $request->email,
```

我们可以对我们的用户更新方法做同样的事情

```
sail artisan make:request Admin/UpdateUserRequest
```

将下面的代码复制到`UpdateUserRequest`上

`app/Http/Requests/Admin/UpdateUserRequest.php`

```
<?php
namespace App\Http\Requests\Admin;
use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Validation\Rules;
class UpdateUserRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }
    /**
     * Get the validation rules that apply to the request.
     *
     * @return array<string, mixed>
     */
    public function rules()
    {
        return [
            'name' => ['required', 'string', 'max:255'],
            'email' => ['required', 'string', 'email', 'max:255', 'unique:users,email,'.$this->user->id],
            'password' => ['nullable','confirmed', Rules\Password::defaults()],
        ];
    }
}
```

打开用户控制器，将 param 请求更新为`UpdateUserRequest`，然后取消对更新方法的验证。确保包括控制器顶部的`use App\Http\Requests\Admin\UpdateUserRequest;`

我们最终的存储和更新方法是在使用表单请求重新构造验证之后。

```
public function store(StoreUserRequest  $request)
{
    $user = User::create([
        'name' => $request->name,
        'email' => $request->email,
        'password' => Hash::make($request->password),
    ]);
    if(! empty($request->roles)) {
        $user->assignRole($request->roles);
    }
    return redirect()->route('user.index')
                    ->with('message','User created successfully.');
}
public function update(UpdateUserRequest $request, User $user)
{
    $user->update([
        'name' => $request->name,
        'email' => $request->email,
    ]);
    if($request->password){
        $user->update([
            'password' => Hash::make($request->password),
        ]);
    }
    $roles = $request->roles ?? [];
    $user->assignRole($roles);
    return redirect()->route('user.index')
                    ->with('message','User updated successfully.');
}
```

下一部分我们将做更多的重构。

[](/how-to-enable-xdebug-on-laravel-sail-and-debugging-code-with-vs-code-872fd750b340) [## 如何在 Laravel Sail 上启用 Xdebug 并用 VS 代码调试代码

### 用 XDebug 和 Visual Studio 代码调试 Laravel

blog.devgenius.io](/how-to-enable-xdebug-on-laravel-sail-and-debugging-code-with-vs-code-872fd750b340) 

Laravel 管理面板可在 https://github.com/balajidharma/basic-laravel-admin-panel 的[获得。安装管理面板并分享您的反馈。](https://github.com/balajidharma/basic-laravel-admin-panel)

感谢您的阅读。

敬请关注更多内容！

【balajidharma.medium.com】跟我来[](https://balajidharma.medium.com/)*。*

*上一部分—第 9 部分:[为管理员用户创建帐户更新页面](https://medium.com/dev-genius/laravel-create-an-account-update-page-for-admin-users-e123cd88f24b)*

*下一部分—第 11 部分:[使用服务&动作类](/restructuring-a-laravel-controller-using-services-action-classes-288b802122b5)重构 Laravel 控制器*