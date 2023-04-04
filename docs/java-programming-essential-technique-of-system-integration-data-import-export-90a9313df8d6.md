# Java 编程——系统集成和数据导入导出的关键技术

> 原文：<https://blog.devgenius.io/java-programming-essential-technique-of-system-integration-data-import-export-90a9313df8d6?source=collection_archive---------7----------------------->

## 使用 Apache Commons CSV 和 OpenCSV 读取/写入 CSV 文件格式的简单方法

![](img/78e57196bde0610c59dad6c7d109bfbe.png)

照片由[加曼·爱丽丝](https://unsplash.com/@gamanalice3012?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

JSON 无疑是 API 和许多其他系统进程的流行数据结构。然而，对开发人员友好的数据结构对最终用户来说并不意味着相同。尽管可以使用文本编辑器查看 JSON，但是对数据的分析工作(比如排序和过滤)肯定不是简单的。

由于 Microsoft Office 和 LibreOffice 等其他类似软件的广泛使用，电子表格中的数据可以很容易地可视化和操作。因此，如果您要为业务用户提取一组系统记录，电子表格格式的数据文件通常是他们的首选。

读写逗号分隔值(CSV)文件的编程技能是必不可少的。大多数人类可读的数据导出或数据导入功能都是 CSV 格式的。尽管普遍使用 JSON 数据格式，但由于其简单性，CSV 仍然是系统集成中常用的文件格式。

例如，让我们看看下面 CSV 格式的产品记录。共有 4 列，第一行是标题

```
**id,category,name,price**
3422,Drinks,Pepsi Max No Sugar Cola Can 24x330ml,48.75
5456,Food Cupboard,HEINZ Cream of Chicken Soup 400g,1
2123,Food Cupboard,Dolmio Pasta Bake Tomato and Cheese Pasta Sauce 500g,2.2
6523,Frozen,Garlic and Cheese Pizza Bread 245g,1.5
```

下面的代码向您展示了如何解析 CSV 文件和用普通 Java 构造数据对象。它只是忽略列标题，并简单地假定每个数据字段的位置是固定的。

显然，硬编码的列位置是不灵活的。如果重新排列了列位置，它将无法处理 CSV 文件。此外，CSV 文件中的数据字段可以用引号括起来。当然，还需要进一步加强。利用现成的库来处理 CSV 解析是一种快速的方法，而不是重新发明轮子。

Apache Commons CSV 和 OpenCSV 是处理 CSV 数据格式的流行库。在本文中，让我们探讨一下这两个库的用法并进行比较。

# Apache Commons CSV

Apache Commons 提供了各种有用的库，使编码之旅更加顺利。 [CSV](https://commons.apache.org/proper/commons-csv/) 是读写 CSV 文件的模块之一。

要将 Apache Commons CSV 添加到您的类路径中，请在 pom.xml 中包含这个 maven 依赖项

```
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-csv</artifactId>
  <version>1.9.0</version>
</dependency>
```

## CSV 文件解析

要读取 CSV 文件并按位置解析列值，请定义 CSV 格式，然后解析该文件。可以通过`CSVRecord.get(<column index>)`检索列值。

CSV 格式支持 RFC4180，RFC 4180 是 CSV 格式的通用标准。

如果 CSV 文件中的列位置被打乱，按位置读取列值可能会导致系统问题。通过列标题获取值是一种更可靠的方法。您可以只将列索引更改为列标题名称，因为 Apache Commons CSV 处理列标题到列索引的映射。

## CSV 文件输出

类似的模式也适用于 CSV 文件输出。使用生成器设置格式对象。除了 CSV 文件解析之外，还应该在格式中指定列标题。然后，用文件写入器和格式对象构造一个`CSVPrinter`。它可用于将 CSV 记录输出到文件中。

## CSV 格式定制

如您所见，上面的例子使用了通用标准`CSVFormat.RFC4180`。该库是通用的，因为`CSVFormat`是高度可配置的，支持各种格式。

下面的示例代码演示了一个属性列表，您可以自定义格式规范，例如分隔符、是否忽略空行等等。

该库带有一组预定义的格式。 [RFC4180](https://www.rfc-editor.org/rfc/rfc4180) 是 CSV 的通用标准。`CSVFormat.*RFC4180*` 附带以下格式设置:

Microsoft Excel 的 CSV 格式与 RFC4180 非常相似。此外，该格式允许缺少列名和重复的标题名。

制表符分隔符字段格式是另一种流行的格式。`CSVFormat.*TDF*` 只是在格式设置中用制表符替换逗号作为分隔符:

# OpenCSV

Apache Commons CSV 处理 CSV 数据格式，而将数据映射到 bean 对象的代码逻辑是必需的。许多开发人员喜欢 OpenCSV 的自动数据映射到 bean 对象的特性。

Maven 依赖性

```
<dependency>
  <groupId>com.opencsv</groupId>
  <artifactId>opencsv</artifactId>
  <version>5.7.1</version>
</dependency>
```

## CSV 文件解析

下面使用的注释通过列位置定义了字段映射:

有了列映射的定义，OpenCSV 就能够解析 CSV 文件并相应地构造 Java bean

或者，下面是通过标题名称绑定字段的注释:

CSV 解析的代码是一样的，但是使用了不同的策略类`HeaderColumnNameMappingStrategy`而不是`ColumnPositionMappingStrategy`

## CSV 文件输出

将产品列表写入 CSV 文件非常简单，实例化一个`BeanToCsv`并写入一个文件即可！

## CSV 格式定制

OpenCSV 也允许定制 CSV 格式。下面是 CSV 解析的属性列表。例如，可以通过将分隔符更新为制表符来支持制表符分隔的值文件。

对于 CSV 文件输出，一些选项支持自定义。下面的示例使用制表符作为分隔符将记录写入文件。

# 最后的想法

CSV 简单且广泛用于系统集成和数据导出/导入。作为一名软件工程师，对 CSV 数据读/写进行编码是你不应该忽视的基础知识。Apache Commons CSV 和 OpenCSV 都是访问 CSV 文件的便利工具。这些库非常灵活，支持许多 CSV 格式的修改版本。如果您正在寻找一个简单的解决方案，OpenCSV 是一个不错的选择，因为它支持带注释的字段绑定。然而，在定制 CSV 格式方面，Apache Commons CSV 显然更具可配置性。如果对特定数据格式的支持是一个重要的需求，那么它绝对是首选。