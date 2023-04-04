# Python 日志记录基础

> 原文：<https://blog.devgenius.io/python-logging-basics-458ad969e850?source=collection_archive---------2----------------------->

## 如何使用标准日志模块在 Python 中记录消息

![](img/023443592ba612329d20003dc56c633f.png)

照片由[克里斯里德](https://unsplash.com/@cdr6934?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

每个 Python 安装的标准库都包含一个日志模块，为记录消息提供了一个灵活的框架。大多数 Python 应用程序和库都使用这个模块。

日志记录时使用四种类型的类。

*   记录器—公开应用程序可以用来记录日志的接口
*   处理程序—将日志消息发送到适当的目标
*   过滤器—确定将哪些消息发送到哪些输出
*   格式化程序—消息在最终输出中是如何格式化的

# 记录器

要在您的应用程序或库中创建一个记录器，您可以调用`logging.getLogger("app")`。参数`"app"`是您为这个记录器选择的名称。您可以使用一个日志记录器的名称，通过在名称中使用点来形成一个层次结构，如`'app.module.function'`。过滤器可以使用这种层次结构来仅查看来自特定功能或模块的消息。您可以自由选择任何名称或层级。

如果不指定记录器的名称，`logging.getLogger()`将返回一个记录器，它是层次结构的根记录器。

## 日志记录级别

与所有日志框架一样，您可以在单独的级别上记录消息，以改变其重要性。Python 中的日志模块定义了六个级别。

*   关键— `logger.critical("critical message")`
*   错误— `logger.error("error message")`
*   警告— `logger.warn("warn message")`
*   信息— `logger.info("info message")`
*   调试— `logger.debug("debug message")`
*   NOTSET

默认情况下，日志记录级别在日志记录模块上设置为 NotSet。使用 NOTSET 会导致处理所有消息。如果将日志级别设置为 INFO，它将包括 INFO、WARNING、ERROR 和 CRITICAL 消息。不包括 NOTSET 和 DEBUG 消息。

# 经理人

如果您运行前面的示例，日志消息将不会显示。尽管日志记录级别设置为调试，而我们是在信息级别进行日志记录。这是因为我们没有指定处理程序。

日志模块中有几个可用的处理程序，如 StreamHandler、FileHandler、NullHandler、RotatingFileHandle、SocketHandler、DatagramHandler。

我们将使用其中的两个，用于记录控制台的 StreamHandler 和用于记录文件的 FileHandler。参见 [Python 文档](https://docs.python.org/3/library/logging.handlers.html#module-logging.handlers)了解更多关于其他处理程序的细节。

## StreamHandler

对于登录到控制台，我们可以使用 StreamHandler。我们使用`addHandler`方法将处理程序添加到记录器中。

如果运行之前的程序，控制台上会显示日志消息`"Starting application"`。

## 文件处理器

与我们添加 StreamHandler 的方式相同，您可以添加 FileHandler 来启用对文件的日志记录。FileHandler 接受四个参数。

```
**FileHandler**(*filename*, *mode='a'*, *encoding=None*, *delay=False*)
```

第一个参数指定了用于存储日志消息的文件名。第二个参数指定文件模式。如果未指定，则使用模式`'a'`。encoding 参数用于打开具有指定编码的文件。如果 delay 为真，文件将一直打开，直到第一条消息必须写入文件。不然，是不是直接开了。

如果你打开文件`logging.txt`，你会看到一行信息“启动应用程序”。通常，在记录日志时，您希望在每个日志行上看到额外的信息，比如日期和时间。

这就是我们可以使用格式化程序的地方。

# 格式化程序

格式化程序负责将内部日志记录转换成可以被人或外部系统解释的字符串。您可以将格式字符串与默认格式化程序一起使用。如果你不提供一个格式字符串，它就像前面的例子一样使用日志消息。

如果您想为每个输出消息添加时间戳，您必须创建一个格式化程序实例，并在处理程序上设置它。

在下面的示例中，我在第六行创建了一个新的格式化程序，并在 StreamingHandler 上设置了该格式化程序。这意味着日志模块将使用这个格式化程序来格式化控制台上的消息。FileHandler 仍然有默认的格式化程序。

应用程序启动时会在控制台中显示以下消息。

```
2020-06-14 17:49:34,614 - INFO - Starting application
```

格式化程序的第二个参数可用于指定日期和时间的特定格式。

日志框架的最后一部分是过滤器。

# 过滤

您可以使用筛选器实现比使用级别提供的更复杂的筛选。过滤器可以附加到处理程序和记录器上。附加到处理程序的筛选器在处理程序发出事件之前使用。每当在将事件发送到处理程序之前记录事件时，都会使用附加到记录器的筛选器。

默认过滤器只有一个参数`name`，这允许您设置记录器的名称来应用该过滤器。使用命名层次结构时，您可以应用更精细的过滤。例如，用“A.B”初始化的过滤器将允许记录器“A.B”、“A.B.C”、“A.B.C.D”、“A.B.D”等记录事件。但不是“A.BB”、“B.A.B”等。如果用空字符串初始化，所有事件都将被传递。

# 添加格式化程序和处理程序

因为添加格式化程序和处理程序非常常见，所以日志模块提供了助手函数`basicConfig`。该函数通过创建一个默认为`Formatter`的`StreamHandler`并将其添加到 root logger 来执行基本配置。

# 结论

本文展示了使用标准日志模块在 Python 中进行日志记录的基础。拥有标准日志库的好处是大多数 Python 模块都使用它。因此您的应用程序日志可以包含您自己的消息，这些消息与来自第三方模块的消息集成在一起。

感谢您的阅读。