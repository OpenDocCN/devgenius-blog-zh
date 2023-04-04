# Laravel 8 创建自定义助手函数示例

> 原文：<https://blog.devgenius.io/laravel-8-create-custom-helper-function-example-62ad587f4444?source=collection_archive---------2----------------------->

今天，我们将看到 laravel 8 创建自定义助手函数示例，众所周知，laravel 在其框架中提供了许多现成的 mate 函数，但很多时候我们需要在项目中使用我们自己的自定义函数来创建自定义助手函数，所以，我在这里向您展示 laravel 8 中的自定义助手函数示例。

所以，让我们开始看看如何在 laravel 8 中创建自定义助手。

> *第一步:创建 helpers.php 文件*

在这一步中，我们将在 laravel 项目中创建 app/helpers.php 文件，并添加以下代码。

**app/helpers.php**

```
<?phpfunction change_Date_Format($date,$format){
    return \Carbon\Carbon::createFromFormat('Y-m-d', $date)->format($format);    
}?>
```

> *第二步:在 composer.json 文件中添加 Helper 文件路径*

在 **composer.json** 文件中添加以下代码。

```
"autoload": {
        "psr-4": {
            "App\\": "app/",
            "Database\\Factories\\": "database/factories/",
            "Database\\Seeders\\": "database/seeders/"
        },
         "files": [
            "app/helpers.php"
        ]
    },
```

> *第三步:运行命令*

现在，在您的终端中运行以下命令

```
composer dump-autoload
```

所以，我们已经完成了 laravel 8 中的自定义帮助函数，现在我们可以在任何地方使用这个函数。

这里我在刀片文件中使用这个函数。

```
<html>
 <head>
     <meta charset="utf-8">
     <meta http-equiv="X-UA-Compatible" content="IE=edge">
     <title>Laravel 8 Create Custom Helper Functions Example - websolutionstuff.com</title>
     <link rel="stylesheet" href="">
 </head>
 <body>
    <h3>Laravel 8 Create Custom Helper Functions Example - websolutionstuff.com</h3>
     <h3>New Date Format: {{ change_Date_Format(date('Y-m-d'),'m/d/Y')  }}</h3>
 </body>
</html>
```

我们将得到如下所示的输出。

![](img/170f5979d2a24a2a10198f4351278a64.png)

Laravel 8 创建自定义助手函数示例

 [## Laravel 中的 Cron 作业调度

### 今天我将向您展示 Laravel 中的 Cron 作业调度，很多时候我们需要运行一些特定间隔的代码…

websolutionstuff.medium.com](https://websolutionstuff.medium.com/cron-job-scheduling-in-laravel-2361f0fd13dd)  [## 使用 Laravel Tinker 创建虚拟数据

### 在这个例子中，我们可以看到如何使用 tinker 和 factory 一次在数据库中添加多个虚拟记录，主要是…

websolutionstuff.medium.com](https://websolutionstuff.medium.com/create-dummy-data-using-laravel-tinker-e9bea37acbf2)  [## Laravel 8 CRUD 操作示例

### 你好朋友，

websolutionstuff.medium.com](https://websolutionstuff.medium.com/laravel-8-crud-operation-example-42355424049) 

感谢阅读…！！

别忘了喜欢，分享和评论。