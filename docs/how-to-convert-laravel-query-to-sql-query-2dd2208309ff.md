# 如何将 Laravel 查询转换成 SQL 查询

> 原文：<https://blog.devgenius.io/how-to-convert-laravel-query-to-sql-query-2dd2208309ff?source=collection_archive---------4----------------------->

在本文中，我们将了解如何将 laravel 查询转换为 SQL 查询。很多时候，我们需要 laravel query builder 将其原始 SQL 查询作为一个字符串。

Laravel 提供了不同的方法来获取原始 SQL 查询。您可以使用 toSql()查询构建器方法获得 SQL 查询。

此外，您可以使用 laravel 查询日志来获取 laravel 中的 SQL 查询。为了获取查询日志，我们将使用 enableQueryLog()方法。

因此，让我们看看如何从一个 laravel 中获取原始 SQL 查询。

**toSql()函数:**

在这个例子中，我们将使用 toSql()方法来获取原始 Sql 查询。toSql()获取查询的 Sql 表示并返回字符串值。如果您的查询更复杂或者有子查询，这个方法不会显示整个查询。

**SQL 查询示例:**

```
SELECT * FROM articles WHERE status='published';
```

**Laravel 查询示例:**

```
$articles = Article::where('status', 'published')->get();
```

使用`toSql()`方法，您可以用`toSql().`更改上面查询中的`get()`部分

```
$article = Article::where('status', 'published')->toSql();
```

**输出:**

```
"select * from `articles` where `status` = ?"
```

**另请阅读:** [**如何获取 Laravel 9**](https://websolutionstuff.com/post/how-to-get-client-ip-address-in-laravel-9) 中的客户端 IP 地址

**enableQueryLog()函数:**

Laravel 可以有选择地将针对当前请求运行的所有查询记录到内存中。您可以使用 **enableQueryLog()方法来启用日志。**

```
public function store(Request $request){
    \DB::enableQueryLog();
    Post::where('is_active', '=', '1')        
        ->orderBy('publish_date', 'desc')
        ->limit(15)
        ->get();
        dd(\DB::getQueryLog());
}
```

**输出:**

```
array:1 [▼
  0 => array:3 [▼
    "query" => "select * from `posts` where `is_active` = ? order by `publish_date` desc limit 15"
    "bindings" => array:1 [▶]
    "time" => 7.03
  ]
]
```

所以，你可以用数组格式得到查询和查询的执行时间。

```
\DB::enableQueryLog(); // Enable query log

// Your Eloquent query executed by using get()

dd(\DB::getQueryLog()); // Show results of log
```

**你可能也会喜欢:**

*   **阅读也:** [**如何在 MySQL**](https://websolutionstuff.com/post/how-to-get-last-15-days-records-in-mysql) 中获取最近 15 天的记录
*   **阅读另:** [**如何在 MySQL 中使用 Node.js**](https://websolutionstuff.com/post/how-to-import-csv-file-in-mysql-using-node-js) 导入 CSV 文件】
*   **阅读也:** [**如何使用 MySQL 数据库中的 JSON 数据字段**](https://websolutionstuff.com/post/how-to-use-json-data-field-in-mysql-database)
*   **阅读也:** [**如何修复 MySQL 在 XAMPP 意外关机**](https://websolutionstuff.com/post/how-to-fix-mysql-shutdown-unexpectedly-in-xampp)

如果这篇文章有帮助，请点击拍手👏下面的按钮。