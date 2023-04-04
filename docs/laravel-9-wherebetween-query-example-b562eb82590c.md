# Laravel 9 whereBetween 查询示例

> 原文：<https://blog.devgenius.io/laravel-9-wherebetween-query-example-b562eb82590c?source=collection_archive---------14----------------------->

在本文中，我们将看到 laravel 9 whereBetween 查询示例。whereBetween()方法用于检查两列中的值或给定范围的值。

我们将在 laravel 9 中学习 wherebetween 查询。`whereBetween`方法验证一列的值在两个值之间。

所以，让我们来看看 laravel 9 wherebetween 查询示例。

**SQL 的语法之间**

`BETWEEN`运算符选择给定范围内的值。这些值可以是数字、文本或日期。

```
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

**例一:**

在这个例子中，我们将检索工资在 10000 到 20000 之间的雇员的所有记录。

```
SELECT * FROM employee
WHERE salary BETWEEN 10000 AND 20000;
```

**Laravel 的 whereBetween()语法**

在 whereBetween()方法中，第一个参数是列名，第二个参数是范围值的数组。

```
whereBetween('Column Name', [Range of Value]);
```

**读也:** [**Laravel 9 或 Where 条件示例**](https://websolutionstuff.com/post/laravel-9-orwhere-condition-example)

**例 2:**

在 laravel 9 中，您可以使用下面的示例查询从给定的范围值中获取数据。

```
$employee = DB::table('employee')
           ->whereBetween('salary', [10000, 20000])
           ->get();
```

**例 3:**

在本例中，我们将使用 whereBetween()从给定的两个日期范围中检索数据。

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Employee;
use Carbon\Carbon;

class RegisterController extends Controller
{
    public function index(Request $request)
    {
        $employee = Employee::whereBetween('joined_at',[$request->join_date,$request->resign_date])->get();

        dd($employee);
    }
}
```

**你可能也会喜欢:**

*   **读也:** [**拉韦勒 8 雄辩的条件**](https://websolutionstuff.com/post/laravel-8-eloquent-wherehas-condition)
*   **读也:** [**Laravel 9 多对多多态关系**](https://websolutionstuff.com/post/laravel-9-many-to-many-polymorphic-relationship)
*   **读也:** [**拉勒韦尔 9 有许多通过关系的例子**](https://websolutionstuff.com/post/laravel-9-has-many-through-relationship-example)
*   **阅读另:** [**如何在 Laravel 9** 中使用队列上传大型 CSV 文件](https://websolutionstuff.com/post/how-to-upload-large-csv-file-using-queue-in-laravel-9)

如果这篇文章有帮助，请点击拍手👏下面的按钮。