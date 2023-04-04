# TIL: Python sqlite3 —使用占位符绑定 SQL 语句中的值

> 原文：<https://blog.devgenius.io/til-python-sqlite3-using-placeholders-to-bind-values-in-sql-statement-62d10338cb6b?source=collection_archive---------5----------------------->

*TIL(“今天我学到了”)是一些更短、研究更少的帖子，我通常会写这些帖子来帮助组织我的想法，巩固我在工作中学到的东西。我把它们贴在这里作为个人档案，当然也让其他人可能从中受益。*

![](img/48c20b07971beabf995411ed385fa708.png)

[大卫·克洛德](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# SQL 语句中的占位符

正如 [Python sqlite3 文档](https://docs.python.org/3/library/sqlite3.html#using-placeholders-to-bind-values-in-sql-queries)所解释的:

> SQL 操作通常需要使用 Python 变量的值。然而，要小心使用 Python 的字符串操作来组装查询，因为它们容易受到 [SQL 注入攻击](https://en.wikipedia.org/wiki/SQL_injection)

像这样组装字符串时会发生这种情况:

```
*# Never do this -- insecure!*
symbol = 'RHAT'
cur.execute("SELECT * FROM stocks WHERE symbol = '**%s**'" % symbol)
```

或者用 f 弦。

文档建议使用*问号*或*命名的*样式:

```
*# This is the qmark style:*
cur.execute("insert into lang values (?, ?)", ("C", 1972))

*# And this is the named style:*
cur.execute("select * from lang where date=:year", {"year": 1972})
```

# 问题

然而，当我尝试后者时，我遇到了麻烦:

```
params = {"table": table, "identifier": identifier}
query = "select * from :table where identifier=:identifier"
cur.execute("query")
```

一个小时的散弹枪调试之后，我意识到这不起作用，因为 DB-API 的参数替换只适用于表达式，而不适用于表名或 SQL 语法的其他部分。

另一方面，这样做很好:

```
params = {"table": table, "identifier": identifier}
query = f"select * from {table} where identifier=:identifier"
cur.execute("query")
```

然而，这个 f 弦对 SQL 注入来说仍然是不安全的:

```
table = "users; DROP TABLE users;"
params = {"table": table, "identifier": identifier}
query = f"select * from {table} where identifier=:identifier"
cur.execute("query")
```

我看到有人声称(例如在 [StackOverflow](https://stackoverflow.com/questions/31277027/how-to-use-placeholders-for-the-column-and-table-names-in-sqlite3-statements-wh) 上)API 会净化这样的输入，但是它没有。果然，您得到一个错误:

```
Traceback (most recent call last):
  File "test.py", line 11, in <module>
    cur.execute(f"select * from {table} where identifier=:identifier", params)
sqlite3.OperationalError: no such table: users
```

但这是因为表`users`实际上已经在执行语句的过程中被删除了。

# 解决办法

就我所见，对此没有明确的解决方案。因此，如果表名可能来自用户输入，那么唯一要做的就是在将用户输入集成到 SQL 语句之前测试其安全性:

```
table = input("Please enter a table")
identifier = input("Please enter an identifier")
if table in ["users", "admins", "groups"]: # specify tables
    params = {"table": table, "identifier": identifier}
    query = f"select * from {table} where identifier=:identifier"
    cur.execute("query")
```

更好的办法是重写应用程序逻辑，避免使用动态表名或显式用户输入…

这似乎是一个代码气味肯定！

你好！👋*我是汤姆。我是一名软件工程师、技术作家和 IT 倦怠教练。如果想取得联系，可以查看*[*https://tomdeneire . github . io*](https://tomdeneire.github.io/)