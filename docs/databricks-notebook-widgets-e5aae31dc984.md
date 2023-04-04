# 数据砖块笔记本部件

> 原文：<https://blog.devgenius.io/databricks-notebook-widgets-e5aae31dc984?source=collection_archive---------2----------------------->

![](img/7f4935cb526bc882818cfa62d283e418.png)

布雷特·乔丹在 [Unsplash](https://unsplash.com/s/photos/widget?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

带参数的可重复使用的数据块笔记本

输入部件是数据块中的实用工具，允许我们向仪表板和笔记本添加参数。小部件 API 提供了创建获取值和删除它们的方法。在这篇博客中，我们将看到如何使用这些 API。

## 帮助

所有小部件的 API 都在`dbutils.widgets`下。它有一个帮助 API，提供所提到的小部件的详细信息或一般信息。

```
dbutils.widgets.help()
dbutils.widgets.help(<"methodName">)
# dbutils.widgets.help("dropdown")
```

## 创造

创建小部件 API 的方法签名如下:

```
dbutils.widgets.<widget_type>(<"name">, <"defaultValue">, <choices>, <"label">)
```

## 类型

Databricks 笔记本有 4 种不同类型的小部件:

*   `text`接受一个字符串作为输入。
*   `dropdown`创建一个带有值的下拉列表。
*   `combobox`顾名思义，这是文本和下拉菜单的组合。用户可以从下拉列表中选择值，也可以输入自己的值。
*   `multiselect`创建一个值列表。用户可以从列表中选择一个或多个值(复选框)。

```
dbutils.widgets.text(name="txt_p1", defaultValue="Hello")
dbutils.widgets.dropdown(name="dd_p1", defaultValue="2022", choices=["2022", "2021", "2020"])
dbutils.widgets.combobox(name="cbx_p1", defaultValue="P1", choices=["P1", "P2", "P3"])
dbutils.widgets.multiselect(name="mst_p1", defaultValue="1", choices=[str(x) for x in range(1, 13)])
```

## 得到

为了获得分配给小部件参数的值，我们使用了`.get()`方法。它接受感兴趣的小部件名称。

```
dbutils.widgets.get(<"widget_name">)
# mst_p1_val = dbutils.widgets.get("mst_p1").split(",")
```

如果我们调用一个有小部件的笔记本，我们需要提供来自原始调用者的那些值。要传递来自另一个笔记本的值，我们有两个选项，如下所示:

```
%run /path/to/notebook $txt_p1="Hey!"
dbutils.notebook.run("path/to/notebook", 60, {"mst_p1": "P2","txt_p1":'Hey!'})
```

## %run vs notebook.run()

*   使用%run 将在相同的环境中运行目标笔记本，因此目标笔记本中的所有参数和变量都将在调用笔记本中可用。此外，调用笔记本中的变量在目标笔记本中也是可用的。
*   使用%run 命令，我们只能传递字符串参数。在 run()中，我们可以传递地图对象。
*   run()提供了笔记本执行的细节，而%run 本身并没有这样做。应当使用`dbutils.notebook.exit("returnValue")`。

## 去除

如果我们决定不再需要某个或所有小部件，我们可以使用以下方法删除它们:

```
dbutils.widgets.remove(<"widget_name">)
dbutils.widgets.removeAll()
```

虽然我提到了使用 Python API 的所有内容，但这些内容在 SQL 中也是可用的，同样的结构也适用，请参见下面的示例:

```
CREATE WIDGET TEXT txt_p1 DEFAULT 'Hello';CREATE WIDGET DROPDOWN dd_p1 DEFAULT '2022' CHOICES SELECT DISTINCT t_year FROM demo_widget_table;CREATE WIDGET COMBOBOX cbx_p1 DEFAULT 'P1' CHOICES SELECT DISTINCT product_name FROM demo_widget_table LIMIT 5CREATE WIDGET MULTISELECT mst_p1 DEFAULT '1' CHOICES SELECT DISTINCT t_month_number FROM demo_widget_tableSELECT * FROM demo_widget_2 
WHERE transaction_year = getArgument("dd_p1") 
#alternative (= '%$dd_p1%')REMOVE WIDGET <name>
```

默认情况下，widget 设置通常处于**运行笔记本**模式。这将意味着每次我们更改值(选择)，它将运行我们笔记本中的每个单元格。为了避免这种情况，我们需要更改小部件的设置。我们可以**运行访问的命令。**

希望对你有用。快乐记事本！！