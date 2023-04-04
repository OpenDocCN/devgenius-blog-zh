# 使用正则表达式的 Laravel 9 电话号码验证

> 原文：<https://blog.devgenius.io/laravel-9-phone-number-validation-using-regex-3549bb81cbfc?source=collection_archive---------4----------------------->

在本文中，我们将看到使用 regex 的 laravel 9 电话号码验证。在 laravel 9 中，我们将看到验证电话号码的不同方法。

此外，我们将在 laravel 9 中看到一个 10 位数电话号码验证的例子，以及使用 regex 或正则表达式的手机号码验证。

因此，让我们看看如何在 laravel 9 中验证电话号码，以及在 laravel 9 中验证电话号码正则表达式。

Laravel 提供了几种不同的方法来验证应用程序的输入数据。

最常见的是对所有传入的 HTTP 请求使用可用的`validate`方法。验证规则被传递到`validate`方法中。

了解更多关于 laravel 验证:[**Laravel 9**](https://laravel.com/docs/9.x/validation)**官方文件。**

**示例 1:如何在 Laravel 8/9 中验证手机号码/电话号码**

在本例中，我们将创建路由和控制器，并在 laravel 应用程序的 validate 方法中添加 10 位数字验证规则。

**另请阅读:** [**如何使用 jQuery**](https://websolutionstuff.com/post/how-to-validate-phone-number-using-jquery) 验证电话号码

**第一步:添加路线**

在这一步中，我们将在 web.php 文件中添加以下路由。

**路线/网页. php**

```
use App\Http\Controllers\PostController;

Route::get('/post/create', [PostController::class, 'create']);
Route::post('/post', [PostController::class, 'store']);
```

**第二步:创建控制器**

现在，我们将创建两个函数，并将以下代码添加到控制器文件中。

在存储函数中，我们将添加验证，该验证必须具有值的精确长度。此外，您可以根据需要使用最小或最大长度验证。

**app/Http/Controllers/post controller . PHP**

```
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;

class PostController extends Controller
{
    /**
     * Show the form to create a new blog post.
     *
     * @return \Illuminate\View\View
     */
    public function create()
    {
        return view('post.create');
    }

    /**
     * Store a new blog post.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $validated = $request->validate([
            'phone_number' => 'required|numeric|digits:10'
        ]);
    }
}
```

验证规则被传递到`validate`方法中。所有可用的验证规则都被 [**记录在**](https://laravel.com/docs/9.x/validation#available-validation-rules) 中。

*   如果验证规则通过，您的代码将继续正常执行；
*   然而，如果验证失败，将抛出一个`Illuminate\Validation\ValidationException`异常，并且正确的错误响应将自动发送回用户。

**另请阅读:** [**如何在 Laravel 9**](https://websolutionstuff.com/post/how-to-create-unique-slug-in-laravel-9) 中创建独特的 Slug

**示例 2:在 Laravel 中使用 Regex 进行手机号码验证**

在本例中，我们将使用正则表达式或 regex 来验证手机号码。

```
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\Request;

class PostController extends Controller
{
    /**
     * Show the form to create a new blog post.
     *
     * @return \Illuminate\View\View
     */
    public function create()
    {
        return view('post.create');
    }

    /**
     * Store a new blog post.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $validated = $request->validate([
            'phone_number' => 'required|regex:/^([0-9\s\-\+\(\)]*)$/|min:9'
        ]);
    }
}
```

**您可能还会喜欢:**

*   **读也:**[Laravel 9 表单验证示例](https://websolutionstuff.com/post/laravel-9-form-validation-example)
*   **读也:** [**Laravel 9 CRUD 操作示例**](https://websolutionstuff.com/post/laravel-9-crud-operation-example)
*   **阅读另:** [**Laravel 更新时唯一验证**](https://websolutionstuff.com/post/laravel-unique-validation-on-update)
*   **阅读也:** [**如何使用 Google Recaptcha V2 在 Laravel 9**](https://websolutionstuff.com/post/how-to-use-google-recaptcha-v2-in-laravel-9)

如果这篇文章有帮助，请点击拍手👏下面的按钮。