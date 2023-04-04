# 如何从 shell 脚本处理和更新 XML 有效负载

> 原文：<https://blog.devgenius.io/how-to-process-and-update-xml-payloads-from-the-shell-script-2631c60fefe4?source=collection_archive---------0----------------------->

## 了解如何使用 xmlstarlet 在脚本中有效地处理 XML 负载

![](img/a595c2e72b74992285c7fcc10b996177.png)

沙哈达特·拉赫曼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果您从事 IT 工作，或者甚至将它视为您的主要爱好之一，那么您在某个时候已经被编写了一个 shell 脚本。如果你也从事业务运营方面的工作，这可以成为你的日常任务。创建、维护或升级现有流程。

如今，与外部系统进行交互更加常见，这些外部系统使用 XML 文件中的有效负载，甚至使用这种格式编写的配置文件。

原生 shell 脚本并没有提供一种简单的方法来实现这一点，也没有像 Python、Java 或 Go 等现代编程语言那样支持库来处理这一点。因此，您可能已经发现自己正在编写代码来解析这种有效载荷。但这不是唯一的方法，我们可以(也应该！)利用现有的工具为我们完成这项工作。

# xmlstarlet

我找不到更好的方法来解释 xmlstarlet 做了什么，所有者在他们的源代码库中做了定义:

> XMLStarlet 是一个命令行 XML 工具包，可以使用一组简单的 shell
> 命令来转换、
> 查询、验证和编辑 XML 文档和文件，类似于使用 grep/sed/awk/
> tr/diff/patch 对纯文本文件所做的那样。

因此，xmlstarlet 提供了处理 XML 的所有能力，因为这是纯文本文件。

 [## XMLStarlet 命令行 XML 工具包:新闻

### 亲爱的 XMLStarlet 用户，你可能已经注意到 xmlstarlet 的开发有些停滞了。得到…

xmlstar.sourceforge.net](http://xmlstar.sourceforge.net/) 

## 装置

该实用程序的安装过程非常简单，取决于您使用的操作系统。我假设大多数 shell 脚本都考虑了 Unix 机器目标。安装这个工具非常容易，因为大多数软件包仓库都有这个工具的版本。

因此，如果您使用基于 apt 的系统，您需要执行以下命令:

```
sudo apt-get install xmlstarlet 
```

如果您使用的是另一个平台，请不要担心，因为他们有所有最常用的操作系统和平台的可用版本，您可以在下面的链接中看到:

[](http://xmlstar.sourceforge.net/download.php) [## XMLStarlet 命令行 XML 工具包:下载

### 来源:可在 SourceForge Linux 上获得:与您最近的 Linux 发行版捆绑在一起:可在 SourceForge 上获得…

xmlstar.sourceforge.net](http://xmlstar.sourceforge.net/download.php) 

## 使用

一旦我们安装了这个软件，我们要做的第一件事就是启动它来查看可用的选项。

```
XMLStarlet Toolkit: Command line utilities for XML
Usage: D:\Data\Downloads\xmlstarlet-1.6.1-win32\xmlstarlet-1.6.1\xmlstarlet.exe [<options>] <command> [<cmd-options>]
where <command> is one of:
  ed    (or edit)      - Edit/Update XML document(s)
  sel   (or select)    - Select data or query XML document(s) (XPATH, etc)
  tr    (or transform) - Transform XML document(s) using XSLT
  val   (or validate)  - Validate XML document(s) (well-formed/DTD/XSD/RelaxNG)
  fo    (or format)    - Format XML document(s)
  el    (or elements)  - Display element structure of XML document
  c14n  (or canonic)   - XML canonicalization
  ls    (or list)      - List directory as XML
  esc   (or escape)    - Escape special XML characters
  unesc (or unescape)  - Unescape special XML characters
  pyx   (or xmln)      - Convert XML into PYX format (based on ESIS - ISO 8879)
  p2x   (or depyx)     - Convert PYX into XML
<options> are:
  -q or --quiet        - no error output
  --doc-namespace      - extract namespace bindings from input doc (default)
  --no-doc-namespace   - don't extract namespace bindings from input doc
  --version            - show version
  --help               - show help
Wherever file name mentioned in command help it is assumed
that URL can be used instead as well.

Type: xmlstarlet <command> --help <ENTER> for command help
```

因此，我们需要决定的第一件事是我们需要使用哪个命令，这些命令与我们想要执行的操作是一一对应的。在这篇文章中，我将集中讨论主要的命令，但是正如你所看到的，这包括从选择 XML 值，更新它，甚至验证它。

## 用例 1:选择一个值。

我们将从最简单的用例开始，尝试从 XML (file.xml)中选择一个值，如下所示:

```
<root>
<object1 name="attribute_name">value in XML</object1></root>
```

因此，我们将从最简单的命令开始:

```
./xmlstarlet  sel  -t -v "/root/object1"  ./file.xml *value in XML*
```

这提供了 object1 元素内部的值，使用-t 定义一个新模板，使用-v 指定句子的值。现在，如果我们想获得属性值，我们可以像前面的命令那样做:

```
**./xmlstarlet  sel  -t -v "/root/object1/@name"  ./file.xml** *attribute_name*
```

## 用例 2:更新值。

现在，我们将遵循基于同一文件的另一种方法。我们将更新 object1 元素的值来设置文本“updated text”。

为此，我们执行以下命令:

```
**./xmlstarlet ed -u "/root/object1" -v "updated text" ./file.xml** *<?xml version="1.0"?>
<root>
  <object1 name="attribute_name">updated text</object1>
</root>*
```

# 摘要

xmlstarlet 将为我们提供管理复杂事物的所有选项，因为有 XML，并且可以简单地完成您可以想象的所有任务，而不需要自己编写所有的解析逻辑。我希望从现在开始，当您需要在 shell 脚本中管理 XML 时，您是一个更快乐的开发人员。