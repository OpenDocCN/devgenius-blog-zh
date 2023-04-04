# laravel 9 where/where notin/or where/or herenotin

> 原文：<https://blog.devgenius.io/laravel-9-wherein-wherenotin-orwherein-orwherenotin-8a44771cfca1?source=collection_archive---------22----------------------->

在本文中，我们将看到 Laravel 9 `whereIn` / `whereNotIn` / `orWhereIn` / `orWhereNotIn`查询示例。

`whereIn`()方法用于在给定的值范围内检查值。因此，我们将学习 laravel 9 `whereIn`查询、laravel 9 `whereNotIn`查询、laravel 9 或 where 查询、laravel 9 或 hereNotIn 查询示例。

`whereIn`方法验证给定列的值是否包含在给定数组中。`whereNotIn`方法验证给定列的值不包含在给定数组中。

**SQL 的语法:**

```
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

**例 1:**

在本例中，我们将检索所有符合 In 条件的雇员的记录。

```
SELECT * FROM employee WHERE emp_id IN (1,2,3)
```

**Laravel 的语法:**

```
whereIn(Coulumn Name, Array of Value)
```

**阅读也:**[**Laravel 9 between 查询示例**](https://websolutionstuff.com/post/laravel-9-wherebetween-query-example)

**例二:**

在这个例子中，我们将使用**where()**方法来检索数据。

```
$employee = DB::table('employee')
                    ->whereIn('id', [1, 2, 3])
                    ->get();
```

**例 3:**

在这个例子中，我们将使用 **whereNotIn()** 方法来检索数据。

```
$employee = DB::table('employee')
                    ->whereNotIn('id', [1, 2, 3])
                    ->get();
```

**例 4:**

在本例中，我们将使用**或 where()**方法检索数据。

```
$employee = DB::table('employee')
                    ->where('role','=','software engineer')
                    ->orWhereIn('emp_id', [1, 2, 3])
                    ->get();
```

**读也:** [**Laravel 9 Where 条件示例**](https://websolutionstuff.com/post/laravel-9-where-condition-example)

**例 5:**

在这个例子中，我们将使用 **orWhereNotIn()** 方法来检索数据。

```
$employee = DB::table('employee')
                    ->where('role','=','software engineer')
                    ->orWhereNotIn('emp_id', [1, 2, 3])
                    ->get();
```

**你可能也会喜欢:**

*   **另请参阅:** [**如何使用 Markdown maillable Laravel 9**](https://websolutionstuff.com/post/how-to-send-email-using-markdown-mailable-laravel-9)发送电子邮件
*   **阅读也:** [**Laravel 9 Foreach 循环变量示例**](https://websolutionstuff.com/post/laravel-9-foreach-loop-variable-example)
*   **读还:** [**Laravel 9 内部连接查询示例**](https://websolutionstuff.com/post/laravel-9-inner-join-query-example)
*   **读也:** [**Laravel 9 AJAX CRUD 示例**](https://websolutionstuff.com/post/laravel-9-ajax-crud-example)

如果这篇文章有帮助，请点击拍手👏下面的按钮。