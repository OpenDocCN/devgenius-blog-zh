# Python 与 MySQL 数据库

> 原文：<https://blog.devgenius.io/python-with-mysql-database-fab55cb56a06?source=collection_archive---------13----------------------->

![](img/26d36ca32ad8da75bf3e48e7b62383ef.png)

Python，这种编程语言可以用于数据库。今天我们要讲的是如何将 python 与世界上最流行的数据库语言 MySQL 进行整合。根据 Stackoverflow 年的调查。

所以首先我希望你的电脑上已经安装了 python。为了能够使用 MySQL，你必须安装一个 MySQL 驱动程序。

将命令行导航到 pip 的位置，并键入以下内容。

```
C:\Users\*Your Name*\AppData\Local\Programs\Python\Python36-32\Scripts>python -m pip install mysql-connector-python
```

现在您已经下载并安装了 MySQL 驱动程序。

要测试这个驱动程序，您必须创建一个 python 文件并导入 MySQL 驱动程序。

```
import mysql.connector
```

所以接下来，我们要在 Python 和 MySQL 之间创建一个连接。为此，您必须使用 MySQL 数据库的用户名和密码。

```
import mysql.connector

mydb = mysql.connector.connect(
  host="localhost",
  user="*yourusername*",
  password="*yourpassword*"
)
```

现在，您可以从 python 文件开始使用数据库。

那么如何从 python 文件创建数据库呢？我们来谈谈这个。要创建一个数据库，你必须在一个 python 文件中编写这样的代码。

```
mycursor = mydb.cursor()

mycursor.execute("CREATE DATABASE mydatabase")
```

cursor 方法允许您遍历数据库，这将只执行一个查询。

您可以检查系统中有哪些数据库，或者检查您创建的数据库是否出现。为此，您必须使用 SQL 中的**显示数据库**查询。

```
mycursor.execute("SHOW DATABASES")

for x in mycursor:
  print(x)
```

那么如何从 Python 创建一个 MySQL 表呢？为此，您必须使用来自 SQL 的**创建表**查询。

```
mycursor.execute("CREATE TABLE customers (name VARCHAR(255), address VARCHAR(255))")
```

要查看创建的表，只需使用 **show tables** 查询。

```
mycursor.execute("SHOW TABLES")

for x in mycursor:
  print(x)
```

做了那些事之后。插入、删除、更新和检索查询与 MySQL 相同。

现在让我们来谈谈 python 中使用 MySQL 的一些重要方法。

# fetchall()方法

此方法用于从上次执行的查询中获取所有行。

```
mycursor.execute(“SELECT * FROM customers”)myresult = mycursor.fetchall()for x in myresult:
 print(x)
```

输出:

```
(1, 'John', 'Highway 21')
(2, 'Peter', 'Lowstreet 27')
(3, 'Amy', 'Apple st 652')
(4, 'Hannah', 'Mountain 21')
(5, 'Michael', 'Valley 345')
(6, 'Sandy', 'Ocean blvd 2')
(7, 'Betty', 'Green Grass 1')
```

# fetchone()方法

该方法只返回已执行查询结果的第一行。

```
mycursor.execute("SELECT * FROM customers")

myresult = mycursor.fetchone()

print(myresult)
```

输出:

```
(1, 'John', 'Highway 21')
```

# commit()方法

此方法用于更新表。否则，该表不会更新。

```
sql = "UPDATE customers SET address = 'Canyon 123' WHERE address = 'Valley 345'"

mycursor.execute(sql)

mydb.commit()
```

> "你可以拥有没有信息的数据，但你不能拥有没有数据的信息."—丹尼尔·凯斯·莫兰

*更多内容尽在*[*blog . dev genius . io*](http://blog.devgenius.io)*。*