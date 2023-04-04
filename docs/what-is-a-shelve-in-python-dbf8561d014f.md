# Python 中的 shelve 是什么？

> 原文：<https://blog.devgenius.io/what-is-a-shelve-in-python-dbf8561d014f?source=collection_archive---------7----------------------->

如何使用搁置对象？

![](img/c2f3f45201adc7264969fa1e357dd117.png)

搁置对象

幸运的是，对于这个问题，我们有一个简单的答案，shelve 对象非常类似于 Python 中的字典，但唯一的区别是 shelve 将其数据存储在磁盘上的文件中，而不是内存中。

# 有什么好处？

其中一些是:

*   首先，您没有可用 RAM 数量的限制。
*   第二，你可以像在字典里一样方便地使用一个键。
*   第三，你可以在大文件中读写数据，而不用读写整个文件。

这些方面在对大文件执行多次访问的应用程序中非常有用，因为节省的时间是惊人的。

# 如何使用搁置对象？

在 Python 中使用 shelve 对象非常简单，我们只需在需要时遵循以下步骤:

## 1.导入搁置模块

这是一个简单的步骤，我们只用一行代码就可以完成:

```
import shelve
```

## 2.打开搁置对象

该操作允许我们打开或创建(如果文件不存在)shelve 对象或文件，我们可以使用 *shelve.open* 函数执行该任务，将文件名作为参数发送。

```
repos = shelve.open("github_repos")
```

## 3.添加数据条目

现在，我们需要向之前创建的 shelve 对象(repos 对象)添加数据。然而，在这一步我们有一个*限制*，不像字典，shelve 键不能是任何数据类型，在这个例子中*我们必须使用字符串作为键*。

```
repos['SimpleBuy'] = ('hgodinez89', 'Online store', '1.0')repos['login-app'] = ('hgodinez89', 'Login', '1.0')
```

## 4.检索数据条目

要检索以前创建的数据条目，我们只需要用特定的键访问它。

```
repos['login-app']Output:('hgodinez89', 'Login', '1.0')
```

## 5.更新数据条目

当我们需要更新以前创建的数据条目时，我们只需用特定的键访问它。

```
repos['login-app'] = ('hgodinez89', 'Login with Firebase', '1.0')
```

## 6.关闭搁置对象

最后，我们不能忘记关闭我们正在处理的 shelve 对象( *repos* object)，原因是有时需要关闭 shelve 对象，以便将您所做的更改写回磁盘。

```
repos.close()
```

在这里你可以找到之前写的[代码的要点。](https://gist.github.com/hgodinez89/c2f6cfd298d046282335fc5ccf99d20f)

需要记住的是，shelve 对象允许基本的字典操作、键分配或查找、 *del* 、中的*以及 *keys* 方法。*

到目前为止，shelve 对象看起来非常酷，但是，我们有一个限制，听起来可能不好， *shelve 对象不适合多用户*数据库，因为*它们不提供对并发访问的控制*。

最后，我认为 shelve 对象对于那些工作涉及存储或访问大文件中的数据片段的人来说是有用的，其好处是无需整个文件即可读取或写入数据片段。同时改善程序的执行，因为有更多的内存可用于程序的其余部分。

请在本文中随意评论任何没有提到的细节。

非常感谢你来到这里。😊 👈

这篇文章是受这本书的启发📖"[快速 Python 之书](https://books.google.co.cr/books/about/The_Quick_Python_Book.html?id=urVEzQEACAAJ&source=kp_book_description&redir_esc=y)"📖如果你想了解这门语言的基本概念，同时想了解其他高级主题，我向你广泛推荐。