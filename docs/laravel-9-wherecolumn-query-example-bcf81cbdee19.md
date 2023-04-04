# laravel 9 where 列查询示例

> 原文：<https://blog.devgenius.io/laravel-9-wherecolumn-query-example-bcf81cbdee19?source=collection_archive---------27----------------------->

在本文中，我们将看到 laravel 9 whereColumn()查询示例。whereCoulmn 方法用于验证两列是否相等。因此，我们将学习拉韦勒 9 中的 where 列。

Laravel 还提供了 orWhereColumn()方法。所以，你也可以在 laravel 中使用 whereColumn()和 orWhereColumn()方法。

所以，我们来看看 laravel 9 whereColumn()并比较 laravel 9 中的两列。

**where column()的语法**

```
whereColumn('column name', 'operator', 'compare column name')
```

**例 1:**

在本例中，我们将检索所有列相等的雇员记录。

```
$employee = DB::table('employee')
                ->whereColumn('first_name', 'last_name')
                ->get();
```

**例二:**

您也可以向`whereColumn`方法传递一个比较运算符。

```
$users = DB::table('users')
                ->whereColumn('updated_at', '>', 'created_at')
                ->get();
```

**例 3:**

您还可以将一个列比较数组传递给`whereColumn`方法。这些条件将使用`and`操作符连接起来。

```
$users = DB::table('users')
                ->whereColumn([
                    ['first_name', '=', 'last_name'],
                    ['updated_at', '>', 'created_at'],
                ])->get();
```

**您可能还喜欢:**

*   **读起来也:**[**Laravel 9 where day/where year/where time 例句**](https://websolutionstuff.com/post/laravel-9-whereday-whereyear-wheretime-example)
*   **读也:**[**Laravel 9 where null/where not null 查询示例**](https://websolutionstuff.com/post/laravel-9-wherenull-wherenotnull-query-example)
*   **另请参阅:** [**如何使用 Laravel 7/8**](https://websolutionstuff.com/post/how-to-send-e-mail-using-queue-in-laravel-7-8) 中的队列发送电子邮件】
*   **阅读也:** [**Laravel 8 Yajra 数据表示例教程**](https://websolutionstuff.com/post/laravel-8-yajra-datatable-example-tutorial)

如果这篇文章有帮助，请点击拍手👏下面的按钮。