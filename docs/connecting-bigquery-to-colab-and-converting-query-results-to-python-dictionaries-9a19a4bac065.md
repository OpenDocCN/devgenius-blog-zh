# 将 BigQuery 连接到 Colab 并将查询结果转换为 Python 字典

> 原文：<https://blog.devgenius.io/connecting-bigquery-to-colab-and-converting-query-results-to-python-dictionaries-9a19a4bac065?source=collection_archive---------0----------------------->

在 Python 中处理 BigQuery 结果的简单方法(适用于 pandaphobes)

![](img/9cffecadba30775bbdbae3545fa9c50c.png)

由[大卫·斯温戴尔](https://unsplash.com/@sailing69?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/sad-panda?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

我喜欢熊猫。它们非常可爱，让人想抱抱，是很好的宠物。

另一方面，我不得不承认我不是熊猫 Python 库的最大粉丝…虽然我确实使用它(而且它非常强大并且被很好地采用)，但我总是发现语法不直观，文档详尽，但对我的大脑来说很费力。

虽然有些函数看起来像是纯粹的魔术(我有点喜欢`read_html`)，但是实现一些基本任务(如过滤或迭代)的语法经常让我运行原生 Python 数据结构。我发现生成的代码更容易阅读，也更简洁，尤其是如果你利用列表或字典理解的话。

无论哪种方式，如果您使用 BigQuery，并且您当前(或潜在未来)的工具包中有 Python，那么 Google Colab 是一个很好的实验工具。然而，将 BigQuery 数据放入 Colab(然后放入字典)并不是一目了然的。但这很简单。让我告诉你怎么做。我假设你能弄清楚如何去 [Google Colab](https://colab.research.google.com/) ，登录并创建一个新的笔记本。

第一步是执行下面的代码块:

```
from google.colab import auth
auth.authenticate_user()
```

这将为您提供一个链接，您需要登录您的谷歌帐户，批准访问并获得验证码。复制并粘贴到 Colab 的框中，按回车键，你就可以开始了。您现在拥有与 BigQuery 相同的权限。现在让我们为我的项目初始化一个客户端，我把它叫做`BQ`:

```
from google.cloud import bigquery
BQ = bigquery.Client(project='myproject')
```

现在，我可以通过定义查询字符串并使用以下语法来查询 BigQuery:

```
query_sql = "SELECT * FROM `myproject.mydataset.mytable`"
bq_response = BQ.query(query=query_sql)
```

太好了！然而，当我检查对象类型或试图通过打印来检查它时，我很容易发现自己被弄糊涂了:

```
type(bq_response)
<class 'google.cloud.bigquery.job.QueryJob'>print(response)
<google.cloud.bigquery.job.QueryJob object at 0x7f2ebec57d50>
```

我可以使用`dir(bq_response)`遍历这些方法和属性，并尝试找到我需要的，或者我可以使用下面的语法从 BigQuery 获得我的响应作为数据帧:

```
bq_response = BQ.query(query=query_sql).to_dataframe()
```

现在，如果我检查类型，它看起来更熟悉:

```
type(bq_response)
<pandas.core.frame.DataFrame>
```

在许多情况下，这可能很好，但是我想将输出作为字典列表来处理。我更熟悉使用原生 Python 数据结构的语法，我喜欢这种完美映射到 BigQuery (list = ARRAY，dict = STRUCT)中嵌套和重复数据类型的方式。这也映射到表和视图内容(每个视图/表是一个数组，每一行实质上是一个包含 STRUCT 的数组元素，STRUCT 包含一组一致的、可空的字段，这些字段具有定义的数据类型)。

如果您开始使用 BigQuery 脚本并且必须将查询结果存储在变量中，理解这一点也非常有帮助。

无论如何，我跑题了，我的目标是提供一种更少熊猫的方法，这需要多一个链式操作(带有一个非默认参数)来将它转换成一个(结构正确的)字典:

```
bq_response = BQ.query(query=query_sql).to_dataframe().to_dict(orient='records')
```

产生了一个漂亮整洁的字典列表，我可以愉快地理解我的心意！

希望这能对一些人有所帮助，我花了一段时间才弄明白这一切，事实上最后证明这很简单。

作为参考，完整的代码(只有 6 行)在这里:

```
from google.colab import auth
from google.cloud import bigqueryauth.authenticate_user()
BQ = bigquery.Client(project='myproject')query_sql = "SELECT * FROM `myproject.mydataset.mytable`"
bq_response = BQ.query(query=query_sql).to_dataframe().to_dict(orient='records')
```

如果您觉得这(以及其他相关材料)有用和/或有趣，请跟我来！

如果您还不是会员，请[加入 Medium](https://jim-barlow.medium.com/membership),从这个活跃、充满活力和激情的数据人社区每月只需 5 美元就能获得无限的故事。也有很多其他人，但是如果你对数据感兴趣，那么这里就是你要去的地方…