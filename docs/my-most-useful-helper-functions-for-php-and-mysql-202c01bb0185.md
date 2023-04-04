# 我对 PHP 和 MySQL 最有用的助手函数

> 原文：<https://blog.devgenius.io/my-most-useful-helper-functions-for-php-and-mysql-202c01bb0185?source=collection_archive---------13----------------------->

## 2 个准备好的语句的包装函数，因此它们可以像普通查询一样容易地使用

![](img/adbe4122affd886a1cdacc00413c3f1c.png)

卡斯帕·卡米尔·鲁宾在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

迄今为止，PHP 最常见的用法是在后端与 MySQL 数据库通信。尽管越来越多的人转向 Node.js 或 Python 等技术，PHP 仍然是后端编程的常见选择。我喜欢 PHP 是因为它的 C 风格语法和易用性。这是我用了 10 多年的东西，也是我个人网站开发项目的首选。当我第一次开始使用 PHP 时，我使用文本文件作为数据库，但很快我就改用 MySQL 了。在使用普通查询一段时间后，我了解到更好的方法是使用预处理语句。

准备好的声明为抵御 SQL 注入攻击提供了最好的安全保障。然而，与普通的查询相比，它们需要更多的步骤来设置和使用，如果您使用大量的查询，这可能会变得很乏味。为了减少编程开销，我创建了两个包装器函数，使得使用准备好的语句就像普通查询一样简单，这节省了大量的工作和时间。

它们是用普通 PHP 编写的，包含在一个可以包含的文件中。因为我主要从事中小型项目，这是一个很大的优势。我不需要担心下载和建立大框架只是为了让 MySQL 更容易使用。

![](img/f77c37c45d738a99e4b4982fbe167185.png)

照片由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

## 调用用户函数数组

PHP 有[*call _ user _ func _ array*](https://www.php.net/manual/en/function.call-user-func-array.php)函数，它允许回调函数用数组中的所有参数调用。它适用于普通函数和类成员函数。我使用的是面向对象风格的 *mysqli* API。之所以需要这个回调函数，是因为 *mysqli_stmt* 类的 [*bind_param*](https://www.php.net/manual/en/mysqli-stmt.bind-param.php) 函数可以有不同数量的参数，这取决于查询。例如查询" *SELECT * FROM table WHERE ID=？*“只有一个？因此在 *bind_param* 函数调用中只需要一个参数，但是像*“INSERT INTO table(ID，name ) VALUES(？, ?)"*就需要两个参数。因此，没有一个简单的包装函数总是接受相同的参数。相反，包装函数还需要能够处理可变数量的参数。

## 履行

My 2 wrapper 函数，便于使用 MySQL 编写语句。

如上所述，因为 *bind_param* 函数可以接受可变数量的参数，所以包装函数也需要能够做到这一点。与 C++中的可变模板不同，由于 PHP 的非严格类型，这很容易做到，只需在参数前添加…。在调用 *call_user_func_array* 之前，我们迭代可变数量的参数并构建函数所需的数组。

然而，在将 *call_user_func_array* 与 *bind_param* 函数一起使用时，有一个很大的注意事项。 *bind_param* 函数需要通过引用传递参数。因此，在构建数组时，需要用对原始参数的引用来填充数组。

包装函数的另一个好处是错误处理。它们检查每个 MySQL 函数的错误并相应地返回一个值，因此您仍然可以检查查询是否成功。

## 用法示例

有无包装函数和普通查询的预准备语句的比较。

有了这些包装器函数，可以像普通查询一样方便地使用预处理语句，并在所有步骤中进行错误检查。它们是用普通 PHP 编写的，不需要任何额外的框架，这使得它们非常适合中小型项目。因为 PHP 的主要用途是与后端数据库的通信，所以会经常使用预处理语句。这意味着拥有好的包装器功能是非常重要的，它将在项目持续期间节省大量的工作和时间。